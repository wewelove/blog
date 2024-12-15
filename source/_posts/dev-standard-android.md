---
title: Android 项目开发规范
date: 2023-08-20 10:27:22
categories:
- 规范
tags:
- Android
---

## 一、代码命名规范

- 基本命名规范
  - 代码中的命名均不能以下划线或美元符号开始,也不能以下划线或美元符号结束
  - 代码中的命名严禁使用拼音与英文混合的方式,更不允许直接使用中文的方式
  - 杜绝完全不规范的缩写,避免望文不知义
- 包名
  - 包名为小写
  - 点分隔符之间有且仅有一个自然语义的英语单词，包名中单词统一使用单数形式
- 类名
  - 类名必须是一个名词，每个单词首字母大写。除了约定俗成的缩写，尽量使用完整单词
  - 实现类如果和接口区分，请在接口名后加Impl
  - 抽象类命名使用Abstract或Base开头
  - 异常类命名使用Exception结尾
  - 测试类命名以它要测试的类的名称开始,以Test结尾
  - 如果使用到了设计模式，建议在类名中体现出具体模式，有利于阅读者快速理解架构设计思想
  - 枚举类名建议带上Enum后缀
- 方法名
  - 动词或动词+名词
  - 采用驼峰命名方式，第一个单词首字母小写，其它单词首字母大写
- 变量名
  - 采用驼峰命名方式，首字母小写，其后单词的首字母大写
  - 内部使用的变量加m前缀
  - 静态变量加s前缀
  - 控件名不需要和 `id` 名一致，采取统一的缩写前缀
- 常量名
  - 使用`static final`前缀，全部大写，单词间用下划线隔开，并且指出完整含义
  - 不推荐使用枚举，推荐在接口中定义常量字段
- 类成员位置顺序：（类中顺序按如下自顶向下）
  - 静态常量
  - 静态变量
  - 成员变量
  - 构造方法
  - 成员方法
  - 静态类方法
  - 内部类以及内部接口

## 二、代码规格

### 2.1、流程控制

- 嵌套 For/If/Try 最大深度 3；
- 在 if/else/for/while/do 语句中必须用 "{" 和 "}" 括起来，避免引起没必要的错误；
- 对于多个的 if/else 尽量把高概率匹配的放在前面，提高性能；
- 循环语句：尽量不要改变循环变量的值；
- switch 语句：对于多分支语句，建议使用 switch 语句，每个条件都要加 break，在每一个 switch 块内，都必须包含一个 default 语句并放在最后；

### 2.2、空行以及换行

单行空行在以下情况使用：

- 类成员间需要空行隔开：例如成员变量、构造函数、成员函数、内部类、静态初始化语句块、实例初始化语句块
- 成员变量之间的空白行不是必需的。一般多个成员变量中间的空行，是为了对成员变量做逻辑上的分组
- 在函数内部，根据代码逻辑分组的需要，设置空白行作为间隔
- 类的第一个成员之前，或者最后一个成员结束之后，用空行间隔（可选）
- 单空行时使用多行空行是允许的，但是不要求也不推荐

水平缩进：

- 使用 Tab 进行控制，推荐使用 Android Studio 默认的规范

## 三、字符编码

一律使用utf8编码

- String：String.getBytes()编码统一使用utf-8，代码文件编码统一使用UTF-8
- URL编码：请使用URLEncoder. encode(s, “utf-8”)方法

## 四、XML 规则

### 4.1、资源命名规则

- 基本命名规范
  - 命名中单词均采用小写
  - 单词之间采用下划线分割
- layout xml资源命名
  - activity或fragment对应的layout资源，一级标识为`activity`或`fragment`，例如：`activity_media_picker.xml`
  - 对于模块资源，一级标示资源类型，二级标示业务类型，比如：`view_login_xxx.xml`
  - 全局共享的资源：二级前缀可省略或使用app，例如：`view_app_xxx.xml`
  - listview或recycleview的item，一级标识为`item`例如：`item_xxx.xml`
- drawable目录的资源：（包含图片以及xml）
  - 现在临时放置在这个文件夹下的图片，后面需要删掉，命名为 `tmp_XXXXX`
  - xml 定义的 drawable 资源放置在这里
  - 其他正式要用的图片，主要放置在 `drawable-xhdpi` 文件当中，其他需要适配的图片文件自行放置在其他对应的目录下面
  - 多个模块公用的图标命名为 `ic_XXXX`，背景图片命名为 `bg_XXXX`
  - 具体模块单独使用的图片命名为 `modulename_XXXX` (如 `userpage_XXXX`)
  - selector 对应的 xml 命名为 `selector_XXXX`
  - shape 对应的 xml 命名为 `shape_XXXX`
  - 文件名最好能描述内容信息，命名模板`property(属性)_function(功能)_color/size（颜色/大小)_state.png`，例如`ic_switch_white_on.png`
- colors
  - 通用的颜色直接使用名称，如`red`，`green`等
  - 部分通用颜色，可以直接添加数字，如灰色（`gray_aa`, `gray_f1`）
  - 根据页面模块分组，注释说明，空行隔开
  - 具体页面相关的颜色，前面带上页面逻辑相关的名称。例如`login_btn_gray_666666`，对于已有色彩采用间接引用，不存在直接引用16进制颜色代码
- dimens
  - 通用的半径、文字大小，可以分别定义成 `radius_4dp`，`text_15sp`等等
  - 其它和具体业务逻辑相关的，前带上页面逻辑相关的名称
  - 对于已经存在的尺寸采用间接引用

### 4.2、ID 命名规则

- 统一使用前缀表明类型，比如`btn_xxx`，仅在布局使用的控件（不在代码中使用），id使用`layout_xxx`前缀
- 命名单词均采用小写，通过下划线分隔
- 命名二级标识页面模块，例如`btn_login_login`
- 缩写前缀列表（欢迎补充）,自定义控件使用字母缩写

| 控件类型       | 缩写前缀 |
| -------------- | -------- |
| View           | v        |
| Button         | btn      |
| ImageView      | img      |
| ImageButton    | imgBtn   |
| TextView       | tv       |
| ListView       | lv       |
| RecycleView    | rv       |
| LinearLayout   | ll       |
| RelativeLayout | rl       |
| FrameLayout    | fl       |

## 五、Gradle规范

### 5.1、build.gradle(Project:ProjectName) 规范

#### 5.1.1 buildscript 代码块

- 在 `dependencies` 声明android gradle plugin的版本为最新版本（只需要保证二级版本为最新版），例如：

```bash
classpath 'com.android.tools.build:gradle:2.x.x'
```

### 5.2、build.gradle(Module:app) 规范

#### 5.2.1 编译相关规范

- sdk版本号以及构建工具版本使用最新的release版本

```css
 compileSdkVersion 2x
 buildToolsVersion "2x.0.0"
```

- 设置代码的编译版本为 1.7，使编译出的包更通用

```undefined
 compileOptions { 
    sourceCompatibility JavaVersion.VERSION_1_7 
    targetCompatibility JavaVersion.VERSION_1_7
 }
```

#### 5.2.2 defaultConfig 代码块规范

- targetSdkVersion，同compileSdkVersion版本
- minSdkVersion 指定成11，android 4.0，原则上不支持4.0以下系统
- ndk无硬性规定，非特殊的情况，只需在lib目录中保留arm架构的so库

#### 5.2.3 signingConfigs 签名规范

debug版本使用默认设置，release版本的签名文件不得出现硬编码密码在build.gradle文件中，请将storePassword、keyAlias以及keyPassword密码放在properties文件中便于管理，示例：



```csharp
signingConfigs {
    debug {
       storeFile file('debug.keystore')
    }
    release {
       // keystore
       storeFile file('release.keystore')

       // keys
       Properties properties = loadProperties('release.properties')

       storePassword properties.get('storepassword')
       keyAlias properties.get('keyalias')
       keyPassword properties.get('keypassword')
   }
}
 
```

#### 5.2.4 buildTypes 代码块规范

- shrinkResources 是否取消无效资源，去除无效资源有利用减少包大小，但会减慢编译速度,建议在release版本中开启
- minifyEnabled在混淆时去除代码中无用的内容，建议在release中开启，包含插件的项目中不允许开启此选项
- zipAlignEnabled 资源包对齐，可提升执行速度，减少内存消耗，release模式下必须打开

#### 5.2.5 dependencies 代码块规范

- 为规避库冲突，请使用远程依赖的模式，指定groupId、artifactid以及版本号
- 需要使用本地依赖的情况，请通过compile files逐个指定文件名，不允许通过compile fileTree指定文件夹
- 本地依赖包需指定groudId，artifactId以及版本号，例如com.facebook.fresco-fresco-0.8.1.jar
- 如无必要，勿增实体：如果你的app没有support，multi包需求，请勿添加依赖
- exclude android库，例如：exclude support包
- 尽可能使用lib，避免module依赖，独立工程可以通过aar或mvn方式导入；原因：
  - rebuild成本过大
  - 构建工具版本混乱
  - 未来涉及插件化非DSL脚本时，复杂度几何上升

### 5.3、版本管理规范

- 使用最新的二级动态版本
- 使用ext拓展，统一定义版本号
- 为提高可读性，建议将名称改为小写缩写，例如



```undefined
ext {
    target_sdk_version = 24
}
```

### 5.4、权限规范

尽量避免Android 6.0敏感规范，例如：

- 手机状态
- 位置信息（建议使用服务端IP定位）
- 读写SD卡（优先使用拓展目录）

## 六、JavaDoc 规范

**遵循原则**

- 每个开放给用户的函数/类，必须在文档中
- 准确描述其中每个参数/成员的取值
- 仅用于内部的函数/类，不要在文档中暴露给用户（可以另编简要的开发者文档作为记录）
- 对于描述，尽可能的详细，全面

**具体实施**

- 类的描述
  - 类的作用
  - 使用样例
- 接口/抽象类
  - 作用
  - 框架提供的实现
- 方法的描述
  - 传递参数的类型，含义
  - 返回值的类型，含义
  - 传递参数值的范围对于程序执行的影响
  - 方法产生的作用