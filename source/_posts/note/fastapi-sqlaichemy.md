---
title: FastAPI + SQLAlchemy 打造生产级 API 的深度指南
abbrlink: de94f3f9
date: 2025-12-05 09:06:46
series: FastAPI
categories:
  - [笔记]
tags:
  - 后端
  - python
  - FastAPI
  - SQLAlchemy
copyright_author: JaylenCoder
copyright_url: https://mp.weixin.qq.com/s/XRjjhacTxfCx9CC787L_jA
deep: 
---

## 基础结构与核心依赖

### 项目结构

为实现高可维护性、高可测试性及清晰的职责分离，推荐使用三层结构：

```bash
.
├── app/
│   ├── api/             # 路由模块 (调用 Service)
│   ├── core/            # 核心配置、Lifespan 管理
│   ├── db/              # 数据库相关 (models.py, database.py)
│   ├── dao/             # 数据访问对象 (DAO/Repository)
│   ├── services/        # 业务逻辑服务
│   ├── main.py          # 应用入口
│   └── schemas/         # Pydantic 数据模型
└── requirements.txt
```

### 核心依赖 (针对 MySQL)

```bash
pip install fastapi "uvicorn[standard]" sqlalchemy "pydantic[settings]"
# 推荐使用 asyncmy
pip install asyncmy
```

## 数据库连接与异步会话管理

### 安全配置详解 (`.env` & Pydantic)

```py
# app/core/config.py
from pydantic_settings import BaseSettings
classSettings(BaseSettings):
    MYSQL_HOST: str = "localhost"
    MYSQL_PORT: int = 3306
    MYSQL_USER: str = "user"
    MYSQL_PASS: str = "pass"
    MYSQL_DB: str = "dbname"
    @property
defDATABASE_URL(self) -> str:
"""动态生成异步连接字符串，使用 asyncmy 驱动"""
returnf"mysql+asyncmy://{self.MYSQL_USER}:{self.MYSQL_PASS}@{self.MYSQL_HOST}:{self.MYSQL_PORT}/{self.MYSQL_DB}"
settings = Settings()
```

### 异步引擎、会话与驱动选择详解

```py
# app/db/database.py
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker
from app.core.config import settings
# 全局实例化异步引擎 (Engine) 及其连接池：连接池是全局共享资源
# 必须创建异步引擎，网上很多帖子用的同步引擎，误人子弟，完全和 fastapi 的异步特性相违背
engine = create_async_engine(
    settings.DATABASE_URL,
    echo=False,
    future=True,
    pool_size=20,
    max_overflow=0
)
# 创建异步会话工厂 (SessionLocal)
AsyncSessionLocal = sessionmaker(
    engine,
    class_=AsyncSession,
    expire_on_commit=False
)
# FastAPI 依赖注入函数：每次请求提供一个唯一的 Session
asyncdefget_db_session() -> AsyncSession:
"""提供一个独立的数据库会话供每个请求使用"""
# async with 结构保证了 Session 在 yield 结束时自动关闭和回滚
asyncwith AsyncSessionLocal() as session:
yield session
```

### 生产级实践：引擎连接的生命周期管理

```py
# app/core/lifespan.py
from contextlib import asynccontextmanager
from fastapi import FastAPI
from app.db.database import engine
@asynccontextmanager
asyncdeflifespan_manager(app: FastAPI):
"""应用的生命周期管理器"""
# --- 【应用启动阶段：连接池预热与验证】 ---
    print("Database Engine Pool starting and validating...")
try:
# 显式建立一次连接，以验证配置并激活连接池
asyncwith engine.connect():
            print("Database connection pool validated successfully.")
pass
except Exception as e:
        print(f"FATAL ERROR: Failed to connect to database at startup: {e}")
yield# 应用开始接受请求
# --- 【应用关闭阶段】 ---
    print("Database Engine Pool disposing gracefully...")
await engine.dispose()
```

> 在 Python 的 `asynccontextmanager` 和 `FastAPI` 的 `lifespan` 中，`yield` 是一条分界线，将生命周期分为**启动阶段**和**关闭阶段**：

✅ `yield` 前后含义（以 `lifespan` 为例）

```py
@asynccontextmanager
asyncdeflifespan(app: FastAPI) -> AsyncIterator[None]:
# ---------- yield 前 ----------
# 应用启动时执行（FastAPI 启动时调用）
    print("应用启动，初始化资源...")
# 例如：创建数据库连接池、加载模型、初始化全局变量等
yield# ------------------------
# ---------- yield 后 ----------
# 应用关闭时执行（FastAPI 退出时调用）
    print("应用关闭，释放资源...")
# 例如：销毁连接池、关闭文件、清理临时数据等
```

🧩 生命周期详解

| 阶段 | 何时执行 | 常见操作 |
| --- |  --- |  --- |
| `yield` 前 | FastAPI 启动时 | 初始化连接池、加载模型、预热缓存 |
| --- |  --- |  --- |
| `yield` 后 | FastAPI 退出时（Ctrl+C、kill、正常关闭） | 销毁连接池、释放资源、持久化数据 |

🧩 为什么用 `yield`？

- `asynccontextmanager` 是一个上下文管理器，通过 `yield` 把函数切为两部分。

- `yield` 之前是 `__aenter__` 逻辑（初始化），`yield` 之后是 `__aexit__` 逻辑（清理）。

- `yield` 期间，FastAPI 主程序持续运行，处理所有请求。

📚 官方文档描述

> 在 `lifespan` 中，`yield` 前的代码在应用启动时运行，`yield` 后的代码在应用关闭时运行。
> ------ FastAPI 官方文档：Lifespan Events(https://fastapi.tiangolo.com/advanced/events/)

## 模型定义与三层结构 CRUD 示例

### Pydantic Schema 定义

```py
# app/schemas/item.py
from pydantic import BaseModel
from datetime import datetime
classItemBase(BaseModel):
    title: str
    description: str | None = None
classItemCreate(ItemBase):
pass
classItemUpdate(ItemBase):
    title: str | None = None
    description: str | None = None
classItemInDB(ItemBase):
    id: int
    created_at: datetime
classConfig:
        from_attributes = True
```

### ORM 模型

```py
# app/db/models.py
from sqlalchemy.ext.asyncio import AsyncAttrs
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column
from sqlalchemy.dialects.mysql import VARCHAR
from sqlalchemy import Integer, DateTime, func
classBase(AsyncAttrs, DeclarativeBase):
pass
classItem(Base):
    __tablename__ = "items"
    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    title: Mapped[str] = mapped_column(VARCHAR(100), index=True)
    description: Mapped[str | None] = mapped_column(VARCHAR(255), nullable=True)
    created_at: Mapped[DateTime] = mapped_column(DateTime, default=func.now())
```

### DAO/Repository 层

```py
# app/dao/item_dao.py
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select, delete
from typing import List, Optional
from app.db.models import Item
classItemDAO:
"""DAO 负责执行 SQL/ORM 操作，不进行事务提交。"""
def__init__(self, session: AsyncSession):
        self.session = session
asyncdefcreate(self, item_data: dict) -> Item:
        db_item = Item(**item_data)
        self.session.add(db_item)
return db_item
asyncdefget_by_id(self, item_id: int) -> Optional[Item]:
returnawait self.session.get(Item, item_id)
asyncdefget_all(self, skip: int = 0, limit: int = 100) -> List[Item]:
        stmt = select(Item).offset(skip).limit(limit)
        result = await self.session.execute(stmt)
return result.scalars().all()
asyncdefupdate(self, db_item: Item, update_data: dict) -> Item:
for key, value in update_data.items():
            setattr(db_item, key, value)
        self.session.add(db_item)
return db_item
asyncdefdelete(self, item: Item):
await self.session.delete(item)
```

### Service 层：业务逻辑与事务控制

```py
# app/services/item_service.py
from fastapi import HTTPException
from sqlalchemy.exc import IntegrityError
from app.dao.item_dao import ItemDAO
from app.schemas.item import ItemCreate, ItemInDB, ItemUpdate
classItemService:
"""Service 层控制业务逻辑和事务边界。"""
def__init__(self, item_dao: ItemDAO):
        self.item_dao = item_dao
asyncdefcreate_new_item(self, item_in: ItemCreate) -> ItemInDB:
        session = self.item_dao.session
try:
            db_item = await self.item_dao.create(item_in.model_dump())
# --- 【事务提交点】 ---
await session.commit()
# ---------------------
await session.refresh(db_item)
return ItemInDB.model_validate(db_item)
except IntegrityError:
# 捕获已知数据库错误并手动回滚
await session.rollback()
raise HTTPException(status_code=400, detail="Item创建失败：数据重复或不完整。")
# 未知错误交给依赖注入的 async with 结构自动回滚。
asyncdefget_item_by_id(self, item_id: int) -> ItemInDB:
        db_item = await self.item_dao.get_by_id(item_id)
if db_item isNone:
raise HTTPException(status_code=404, detail="Item not found")
return ItemInDB.model_validate(db_item)
asyncdefupdate_existing_item(self, item_id: int, item_in: ItemUpdate) -> ItemInDB:
        session = self.item_dao.session
        db_item = await self.item_dao.get_by_id(item_id)
if db_item isNone:
raise HTTPException(status_code=404, detail="Item not found")
        update_data = item_in.model_dump(exclude_unset=True)
ifnot update_data:
return ItemInDB.model_validate(db_item)
try:
            updated_item = await self.item_dao.update(db_item, update_data)
# --- 【事务提交点】 ---
await session.commit()
# ---------------------
await session.refresh(updated_item)
return ItemInDB.model_validate(updated_item)
except IntegrityError:
await session.rollback()
raise HTTPException(status_code=400, detail="Item更新失败：数据校验错误。")
asyncdefdelete_item_by_id(self, item_id: int):
        session = self.item_dao.session
        db_item = await self.item_dao.get_by_id(item_id)
if db_item isNone:
raise HTTPException(status_code=404, detail="Item not found")
try:
await self.item_dao.delete(db_item)
# --- 【事务提交点】 ---
await session.commit()
# ---------------------
except Exception:
await session.rollback()
raise HTTPException(status_code=500, detail="删除过程中发生未知错误。")
```

### API/Router 层：请求入口

```py
# app/api/endpoints/items.py
from fastapi import APIRouter, Depends, status
from sqlalchemy.ext.asyncio import AsyncSession
from typing import List
from app.db.database import get_db_session
from app.schemas.item import ItemCreate, ItemInDB, ItemUpdate
from app.dao.item_dao import ItemDAO
from app.services.item_service import ItemService
router = APIRouter(tags=["items"], prefix="/items")
# Service 依赖注入函数 (工厂模式)
defget_item_service(session: AsyncSession = Depends(get_db_session)) -> ItemService:
"""创建并提供 ItemService 实例，自动注入 Session"""
    item_dao = ItemDAO(session=session)
return ItemService(item_dao=item_dao)
@router.post("/", response_model=ItemInDB, status_code=status.HTTP_201_CREATED)
asyncdefcreate_item_route(
    item_in: ItemCreate,
    item_service: ItemService = Depends(get_item_service)
):
"""创建 Item"""
returnawait item_service.create_new_item(item_in)
@router.get("/{item_id}", response_model=ItemInDB)
asyncdefread_item_route(
    item_id: int,
    item_service: ItemService = Depends(get_item_service)
):
"""读取 Item"""
returnawait item_service.get_item_by_id(item_id)
@router.put("/{item_id}", response_model=ItemInDB)
asyncdefupdate_item_route(
    item_id: int,
    item_in: ItemUpdate,
    item_service: ItemService = Depends(get_item_service)
):
"""更新 Item"""
returnawait item_service.update_existing_item(item_id, item_in)
@router.delete("/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
asyncdefdelete_item_route(
    item_id: int,
    item_service: ItemService = Depends(get_item_service)
):
"""删除 Item"""
await item_service.delete_item_by_id(item_id)
return
```

## 生产级深度解析与避坑指南

### 致命陷阱：为什么不能用同步引擎？ (原理详解)

**重申：** 必须使用 `create_async_engine` 和异步驱动。同步 I/O 将阻塞 ASGI 事件循环，在高并发下造成性能灾难。

### 高效查询：解决 N+1 问题 (关联加载与批量查询)

#### 场景一：物理外键（推荐）

当 `Order.user_id` 字段定义了真正的数据库外键约束时，我们可以直接利用 SQLAlchemy 的 ORM 关系，使用 `selectinload`。

##### 1. ORM 模型 (已定义关系)

我们继续使用之前定义好的，带有 `relationship` 的模型：

```py
# app/db/models.py (User 和 Order - 带有关系)
classUser(Base):
    __tablename__ = "users"
    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    name: Mapped[str] = mapped_column(String(50))
# 关系定义：User 通过 orders 属性获取关联的 Order 列表
    orders: Mapped[List["Order"]] = relationship(back_populates="user") # <--- 关键
classOrder(Base):
    __tablename__ = "orders"
# user_id 带有 ForeignKey 约束
    user_id: Mapped[int] = mapped_column(ForeignKey("users.id")) # <--- 关键
# 关系定义：Order 通过 user 属性获取关联的 User 对象
    user: Mapped["User"] = relationship(back_populates="orders")
# ... 其他字段
```

##### 2. DAO 层实现：`selectinload` 优化（外键场景最佳实践）

```py
# app/dao/user_dao.py (物理外键场景)
from sqlalchemy.orm import selectinload
# ...
classUserDAO:
# ... (init 函数不变)
asyncdefget_users_with_orders_via_selectinload(self, limit: int = 10) -> List[User]:
"""
        [物理外键最佳实践]
        通过 selectinload 预加载关联数据，查询次数优化为 2 次。
        第1次：SELECT * FROM users LIMIT 10
        第2次：SELECT * FROM orders WHERE user_id IN (ID1, ID2, ...)
        """
        print("--- [外键] 使用 selectinload 优化查询 (2 次数据库交互) ---")
        stmt = (
            select(User)
            .options(selectinload(User.orders)) # 核心优化点：预加载 orders 关系
            .limit(limit)
        )
        result = await self.session.execute(stmt)
        users = result.scalars().unique().all()
# 此时访问 user.orders 不会触发新的数据库查询
# for user in users:
#     # print(f"用户 {user.name} 有 {len(user.orders)} 个订单")
        print("--- [外键] 优化查询结束 ---")
return users
```

#### 场景二：逻辑外键（无 ORM 关系）

假设我们有一个 `Log` 模型，它记录了操作者的 `user_id`，但我们**没有在 ORM 中建立关系**，也没有定义数据库外键。我们只有操作者的 ID 列表，需要查询这些 ID 对应的用户数据。

##### 1. ORM 模型 (`Log` - 无关系)

`Log` 模型只有 `user_id` 字段，但没有 `relationship`。

```py
# app/db/models.py (补充 Log 模型 - 模拟逻辑外键)
classLog(Base):
    __tablename__ = "logs"
    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    user_id: Mapped[int] = mapped_column(Integer, index=True) # <--- 关键：无 ForeignKey 约束，无 relationship
    message: Mapped[str]
    created_at: Mapped[DateTime] = mapped_column(DateTime, default=func.now())
```

##### 2. DAO 层实现：`IN` 子句批量查询（逻辑外键场景最佳实践，也可以考虑 join，实际场景二者选其一，能 join 优先 join，这里演示 in 子查询）

在这种情况下，我们不能使用 `selectinload`，但可以通过先提取 ID，再使用 SQL 的 `IN` 子句进行**批量查询**，然后在 Python 中进行高效聚合。

```py
`# app/dao/log_dao.py (逻辑外键场景)
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select, in_
from typing import List, Dict
from app.db.models import Log, User
classLogDAO:
def__init__(self, session: AsyncSession):
        self.session = session
asyncdefget_logs_with_users_via_in_clause(self, limit: int = 10) -> List[Log]:
"""
        [逻辑外键最佳实践]
        通过 IN 子句批量查询，并在 Python 中手动聚合。
        查询次数优化为 2 次。
        """
        print("--- [逻辑外键] 使用 IN 子句批量查询 (2 次数据库交互) ---")
# 1. 第1次查询：查询 Log 列表
        log_stmt = select(Log).limit(limit)
        log_result = await self.session.execute(log_stmt)
        logs: List[Log] = log_result.scalars().all()
# 提取所有不重复的 user_id
        user_ids = {log.user_id for log in logs}
ifnot user_ids:
return logs
# 2. 第2次查询：使用 IN 子句批量查询 User 对象
# 核心优化点：使用 in_()
        user_stmt = select(User).where(User.id.in_(user_ids))
        user_result = await self.session.execute(user_stmt)
# 将 User 结果转换为 {id: User} 的字典，便于 O(1) 查找
        users_map: Dict[int, User] = {user.id: user for user in user_result.scalars().all()}
# 3. Python 内存中聚合：手动将 User 绑定到 Log 对象上（逻辑关联）
# 🚨 注意：这里是手动给 Log 对象添加一个临时的 user 属性
for log in logs:
            log.user = users_map.get(log.user_id) # 逻辑关联
# if log.user:
#     print(f"Log ID: {log.id}, User Name: {log.user.name}")
        print("--- [逻辑外键] 批量查询结束 ---")
return logs
```

#### 总结

| 场景 | 数据库关系 | ORM 关系 | 最佳实践 | 核心技术 | 优点 |
| --- |  --- |  --- |  --- |  --- |  --- |
| **物理外键** | ✅ 有外键约束 | ✅ 定义了 `relationship` | `selectinload` | `stmt.options(selectinload(...))` | 2 次查询，由 ORM 自动管理聚合 |
| --- |  --- |  --- |  --- |  --- |  --- |
| **逻辑外键** | ❌ 无外键约束 | ❌ 未定义 `relationship` | 批量 `IN` 查询 + 手动聚合 | `list(ID).in_(...)` | 2 次查询，适用于非标准关联和大量数据 |

这两种方法都能将查询次数从  降为  次，从而彻底解决异步环境中的 N+1 性能陷阱。

### 数据库迁移（Alembic 异步配置）

> 为使 Alembic 兼容 SQLAlchemy 2.0 的异步引擎和会话，我们必须在 `alembic/env.py` 中使用 `asyncio` 和 `connectable.run_sync()` 来桥接同步的 Alembic 迁移操作与异步的数据库连接。

#### 最佳实践：核心修改与完整示例

在生产环境中，`alembic/env.py` 文件需要做以下调整：

1. **引入异步模块：** 导入 `asyncio` 和 `create_async_engine`。

2. **动态获取 URL：** 从 `alembic.ini` 或其他配置中获取正确的数据库连接 URL（注意必须使用异步驱动，例如 `mysql+asyncmy://...`）。

3. **使用 `run_migrations_online()`：** 将同步迁移逻辑包裹在一个异步函数中，并通过 `asyncio.run()` 执行。

##### 📄 `alembic/env.py` 完整异步配置示例

```py
# alembic/env.py
from logging.config import fileConfig
import asyncio # 引入 asyncio
from sqlalchemy import engine_from_config
from sqlalchemy.ext.asyncio import create_async_engine # 引入异步引擎
from alembic import context
from app.db.models import Base # 导入你的 ORM Base
# ... (其他导入和配置保持不变)
# config 变量由 Alembic 自动提供
# 目标元数据对象（确保导入了你的 Base）
target_metadata = Base.metadata
# ... (其他辅助函数保持不变)
defrun_migrations_offline():
"""离线模式下运行迁移，通常用于生成 SQL 脚本。"""
    url = context.config.get_main_option("sqlalchemy.url")
    context.configure(
        url=url,
        target_metadata=target_metadata,
        literal_binds=True,
        dialect_opts={"paramstyle": "named"},
    )
with context.begin_transaction():
        context.run_migrations()
defdo_run_migrations(connection):
"""实际执行迁移的同步函数，会被 run_sync 包裹。"""
    context.configure(
        connection=connection,
        target_metadata=target_metadata,
# 启用 compare_type=True 才能检测到类型变化
        compare_type=True,
    )
with context.begin_transaction():
        context.run_migrations()
defrun_migrations_online():
"""
    在线模式下运行迁移，使用 SQLAlchemy 异步引擎。
    这是异步配置的核心部分。
    """
# 1. 从 alembic.ini 获取配置，通常包含 sqlalchemy.url
    configuration = context.config.get_section(context.config.config_ini_section)
# 2. 必须使用 create_async_engine 创建异步引擎
# ⚠️ 确保 alembic.ini 中的 URL 是异步的，如 mysql+asyncmy://...
    connectable = create_async_engine(
        configuration['sqlalchemy.url'],
        future=True,
# echo=True # 调试时可以打开
    )
asyncdefrun_async_migrations():
# 3. 在异步连接上执行
asyncwith connectable.connect() as connection:
# 4. 使用 connection.run_sync() 将同步的 do_run_migrations 
#    函数桥接到异步连接，避免阻塞事件循环。
await connection.run_sync(
lambda sync_conn: do_run_migrations(sync_conn)
            )
# 5. 迁移完成后，确保连接池关闭
await connectable.dispose()
# 6. 运行异步函数
    asyncio.run(run_async_migrations())
if context.is_offline_mode():
    run_migrations_offline()
else:
    run_migrations_online()
```

#### 🔑 关键点解释

1. **异步引擎创建：**

   - 我们使用 `create_async_engine` 而非 `create_engine` 来适配异步环境。
   - URL 必须使用异步驱动格式，例如：在 `alembic.ini` 中配置 `sqlalchemy.url = mysql+asyncmy://user:pass@host/dbname`。

2. **`run_async_migrations`：**

   - 这是一个自定义的 `async` 函数，用于管理异步连接的生命周期。
   - `async with connectable.connect() as connection:`：获取一个异步连接，确保在退出时连接被释放。

3. **`connection.run_sync()`：**

   - 这是 SQLAlchemy 异步的核心机制。Alembic 本身是同步的。这个方法允许我们在 **异步连接** 中执行 **同步** 操作 (`do_run_migrations`)，从而安全地执行迁移脚本，而不会在异步引擎上产生问题。

4. **`asyncio.run()`：**

   - 由于 `alembic` 命令是在同步环境中运行的，我们必须使用 `asyncio.run()` 来启动并等待我们的顶层异步函数 `run_async_migrations()` 完成。

这样配置后，就可以在 **FastAPI/SQLAlchemy 2.0** 异步项目中，使用标准的 Alembic 命令（如 `alembic revision --autogenerate -m "..."` 和 `alembic upgrade head`）来管理数据库迁移了。

## 总结

| 实践点 | 生产级要求 (MySQL) | 最佳实践 |
| --- |  --- |  --- |
| DAO 层职责 | 纯数据操作 | 最好不要 在 DAO 层调用 `commit/rollback`，而是在service层调用。 |
| --- |  --- |  --- |
| Service 层职责 | 业务逻辑与事务控制 | 统一在 Service 层调用 `await session.commit()`，并手动处理 IntegrityError回滚。 |
| 引擎/会话 | `create_async_engine` + `AsyncSession` | Engine 全局实例化，`Session` 随请求创建 (DI)。 |
| 生命周期 | 使用 lifespan | 启动时预热并验证连接池；关闭时调用 await engine.dispose()。 |
| 并发安全 | 解决 N+1 | 使用 selectinload 或 JOIN 或 批量 IN 查询。 |
| 安全性 | 分解连接配置 | 避免在日志中输出敏感凭证。 |
