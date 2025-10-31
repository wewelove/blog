---
title: Vue 项目开发规范
abbrlink: f6787be0
series: development-standards
categories:
  - - 笔记
tags:
  - vue
  - vue.js
date: 2023-08-20 10:27:22
---

## 基础规范

### 项目命名

全部采用小写方式， 以中划线分隔。

```javascript
正例：`smart-admin`
反例：`mall_management-system / mallManagementSystem`
```

### 目录、文件命名

目录、文件名 均以 小写方式， 以中划线分隔。

```javascript
正例：`/head-search/`、`/shopping-car/`、`smart-logo.png`、`role-form.vue`
反例：`/headSearch/`、 `smartLogo.png`、 `RoleForm.vue`
```

### 单引号、双引号、分号

- html 中、vue 的 template 中 标签属性 使用 **双引号**
- 所有 js 中的 字符串 使用 **单引号**
- 所有 js 中的代码行换行要用 **分号**

## Vue3 组合式 API 规范

### 使用 setup 语法糖

- 组件必须使用 `setup` 语法糖
- `setup` 大法方便简洁
- 全局都要使用 `setup` 语法糖

### 组合式 Composition API 规范

组件内必须使用模块化思想，把代码进行拆分；
参照 vue3 官方文档对于 Composition Api 的理解： [更灵活的代码组织 (opens new window)](https://cn.vuejs.org/guide/extras/composition-api-faq.html#better-logic-reuse)，组合式 Api，即 Composition API 解决的是让 相互关联的代码在一起，以更方便的组织代码，故我们的代码格式如下：

**即：将相关的 变量和代码 写到一起，并使用 行注释 进行分块**

```js
<script setup>
// 各种需要导入
import xxxxx;
import xxxxx;
import xxxxx;

// -------- 定义组件属性和对外暴露的方法、以及抛出的事件 --------

// -------- 表格查询的 变量和方法 --------

// -------- 批量操作的 变量和方法 --------

// -------- 表单的 变量和方法 --------

</script>
```

举例，分成了两个模块，即：

- 模块1：显示 、隐藏操作的 变量和方法
- 模块2：表单的 变量和方法

**【以下是正确的例子】**

```js
<script setup>
  import { ref, reactive } from 'vue';
  import { message } from 'ant-design-vue';
  import { SmartLoading } from '/@/components/framework/smart-loading';
  import _ from 'lodash';
  import { categoryApi } from '/@/api/business/category/category-api';
  import { smartSentry } from '/@/lib/smart-sentry';

  // emit
  const emit = defineEmits('reloadList');

  //  组件
  const formRef = ref();

  // ------------------------------ 显示 、隐藏操作的 变量和方法------------------------------

  // 是否展示抽屉
  const visible = ref(false);
  // 显示
  function showModal(categoryType, parentId, rowData) {
    Object.assign(form, formDefault);
    form.categoryType = categoryType;
    form.parentId = parentId;
    if (rowData && !_.isEmpty(rowData)) {
      Object.assign(form, rowData);
    }
    visible.value = true;
  }
  // 隐藏
  function onClose() {
    Object.assign(form, formDefault);
    visible.value = false;
  }

  // ------------------------------ 表单的  变量和方法 ------------------------------
  // 查询表单默认值
  const formDefault = {
    categoryId: undefined, // 分类id
    categoryName: '', // 分类名字
    categoryType: 1, // 分类类型
    parentId: undefined, // 父级id
    disabledFlag: false, // 是否禁用
  };
  // 查询表单
  let form = reactive({ ...formDefault });
  // 表单校验规则
  const rules = {
    categoryName: [{ required: true, message: '请输入分类名称' }],
  };

  function onSubmit() {
    formRef.value
      .validate()
      .then(async () => {
        SmartLoading.show();
        try {
          if (form.categoryId) {
            await categoryApi.updateCategory(form);
          } else {
            await categoryApi.addCategory(form);
          }
          message.success(`${form.categoryId ? '修改' : '添加'}成功`);
          emit('reloadList', form.parentId);
          onClose();
        } catch (error) {
          smartSentry.captureError(error);
        } finally {
          SmartLoading.hide();
        }
      })
      .catch((error) => {
        console.log('error', error);
        message.error('参数验证错误，请仔细填写表单数据!');
      });
  }

  defineExpose({
    showModal,
  });
</script>
```

### 模板引用变量 Ref

对于 vue3 中的模板引用 ref，即 ref 是作为一个特殊的 attribute

```html
<input ref="inputRef">
```

我们要求：

- 使用 ref 方法，参数为空 进行声明变量
- 变量必须以 `Ref` 为结尾
- template 中的 ref 也必须以 `Ref` 为结尾

比如上面的例子，我们声明如下

```js
const inputRef = ref();
```

### 变量和方法的注释

在使用 Composition Api 进行代码编写时，我们有效的组织了代码，但是由于 Composition Api 变量和方法会写到一起，这时候注释就变得很有必要 要求：

- 变量必须都加上注释
- 方法必须加上注释 比如

```js
  // 查询 公告 默认值
  const queryFormState = {
    noticeTypeId: undefined, //分类
    keywords: '', //标题、作者、来源
    documentNumber: '', //文号
    createUserId: undefined, //创建人
    deletedFlag: undefined, //删除标识
    createTimeBegin: null, //创建-开始时间
    createTimeEnd: null, //创建-截止时间
    publishTimeBegin: null, //发布-开始时间
    publishTimeEnd: null, //发布-截止时间
    pageNum: 1,
    pageSize: PAGE_SIZE,
  };
  // 查询 公告 请求表单
  const queryForm = reactive({ ...queryFormState });
```

## Vue3 组件规范

### 组件文件名

组件文件名应该为 pascal-case 格式

正例：

```javascript
components
|- my-component.vue
```

反例：

```javascript
components
|- myComponent.vue
|- MyComponent.vue
```

### 父子组件文件名

和父组件紧密耦合的子组件应该以父组件名作为前缀命名 正例：

```javascript
components
|- todo-list.vue
|- todo-list-item.vue
|- todo-list-item-button.vue
|- user-profile-options.vue （完整单词）
```

反例：

```javascript
components
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue
|- UProfOpts.vue （使用了缩写）
```

### 组件属性

组件属性较多，应该主动换行。

正例：

```html
<MyComponent foo="a" bar="b" baz="c"
    foo="a" bar="b" baz="c"
    foo="a" bar="b" baz="c"
 />
```

反例：

```html
<MyComponent foo="a" bar="b" baz="c" foo="a" bar="b" baz="c" foo="a" bar="b" baz="c" foo="a" bar="b" baz="c"/>
```

### 模板中表达式

组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。复杂表达式会让你的模板变得不那么声明式。我们应该尽量描述应该出现的是什么，而非如何计算那个值。而且计算属性和方法使得代码可以重用。

正例：

```js
<template>
  <p>{{ normalizedFullName }}</p>
</template>

// 复杂表达式已经移入一个计算属性
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```

反例：

```js
<template>
  <p>
       {{
          fullName.split(' ').map(function (word) {
             return word[0].toUpperCase() + word.slice(1)
           }).join(' ')
        }}
  </p>
</template>
```

### 标签顺序

单文件组件应该总是让标签顺序保持为 `<template> 、<script>、 <style>`

正例：

```java
<template>...</template>
<script>...</script>
<style>...</style>
```

反例：

```text
<template>...</template>
<style>...</style>
<script>...</script>
```

## Vue Router 规范

### 页面传参

页面跳转，例如 A 页面跳转到 B 页面，需要将 A 页面的数据传递到 B 页面，推荐使用 路由参数进行传参，即 `{query:param}`

正例：

```js
let id = ' 123';
this.$router.push({ name: 'userCenter', query: { id: id } });
```

### path 和 name 命名规范

- path`kebab-case` 命名规范（尽量与 vue 文件的目录结构保持一致，因为目录、文件名都是 `kebab-case`，这样很方便找到对应的文件）
- path 必须以 / 开头，即使是 children 里的 path 也要以 / 开头。如下示例
- 经常有这样的场景：某个页面有问题，要立刻找到这个 vue 文件，如果不用以/开头，path 为 parent 和 children 组成的，可能经常需要在 router 文件里搜索多次才能找到，而如果以/开头，则能立刻搜索到对应的组件
- name 命名规范采用 `KebabCase` 命名规范且和 component 组件名保持一致！（因为要保持 keep-alive 特性，keep-alive 按照 component 的 name 进行缓存，所以两者必须高度保持一致）

```js
// 动态加载
export const reload = [
  {
    path: '/reload',
    name: 'reload',
    component: Main,
    meta: {
      title: '动态加载',
      icon: 'icon iconfont'
    },

    children: [
      {
        path: '/reload/smart-reload-list',
        name: 'SmartReloadList',
        meta: {
          title: 'SmartReload',
          childrenPoints: [
            {
              title: '查询',
              name: 'smart-reload-search'
            },
            {
              title: '执行reload',
              name: 'smart-reload-update'
            },
            {
              title: '查看执行结果',
              name: 'smart-reload-result'
            }
          ]
        },
        component: () =>
          import('@/views/reload/smart-reload/smart-reload-list.vue')
      }
    ]
  }
];
```

## Vue 项目规范

### 目录规范

```text
src                               源码目录
|-- api                              所有api接口
|-- assets                           静态资源，images, icons, styles等
|-- components                       公用组件
|-- config                           配置信息
|-- constants                        常量信息，项目所有Enum, 全局常量等
|-- directives                       自定义指令
|-- i18n                             国际化
|-- lib                              外部引用的插件存放及修改文件
|-- mock                             模拟接口，临时存放
|-- plugins                          插件，全局使用
|-- router                           路由，统一管理
|-- store                            vuex, 统一管理
|-- theme                            自定义样式主题
|-- utils                            工具类
|-- views                            视图目录
|   |-- role                             role模块名
|   |-- |-- role-list.vue                    role列表页面
|   |-- |-- role-add.vue                     role新建页面
|   |-- |-- role-update.vue                  role更新页面
|   |-- |-- index.less                      role模块样式
|   |-- |-- components                      role模块通用组件文件夹
|   |-- employee                         employee模块
```

### api 目录

- api 文件要以 api 为结尾，比如 `employee-api.js`、`login-api.js`，方便查找
- api 文件必须导出对象必须以 `Api` 为结尾，如：`employeeApi`、`noticeApi`
- api 中以一个对象将方法包裹
- api 中的注释，必须和后端 swagger 文档保持一致，同时保留后端作者

正例：

前端： `department-api.js`

```js
import { getRequest, postRequest } from '/@/lib/axios';

export const departmentApi = {
  /**
   * @description: 查询部门列表 
   * @author 王皓
   * @param {*}
   * @return {*}
   */
  queryAllDepartment: () => {
    return getRequest('/department/listAll');
  },

  /**
   * @description: 查询部门树形列表
   * @author 王皓
   * @param {*}
   * @return {*}
   */
   queryDepartmentTree: () => {
    return getRequest('/department/treeList');
  },

  /**
   * @description: 添加部门 
   * @author 王皓
   * @param {*}
   * @return {*}
   */
  addDepartment: (param) => {
    return postRequest('/department/add', param);
  },
  /**
   * @description: 更新部门信息
   * @author 王皓
   * @param {*}
   * @return {*}
   */
  updateDepartment: (param) => {
    return postRequest('/department/update', param);
  }
};
```

### assets 目录

assets 为静态资源，里面存放 images, styles, icons 等静态资源，静态资源命名格式为 kebab-case

```text
|assets
|-- icons
|-- images
|   |-- background-color.png
|   |-- upload-header.png
|-- styles
```

### components 目录

此目录应按照组件进行目录划分，目录命名为 kebab-case，一个组件必须一个单独的目录 ；

- 一个组件一个目录是为了将来组件的扩展，因为这是整个项目公用的组件；
- 组件入口必须为 index.vue，原因也是因为这是整个项目公用的组件；

举例如下：

```text
|components
|-- error-log
|   |-- index.vue
|   |-- index.less
|-- markdown-editor
|   |-- index.vue
|   |-- index.js
|-- kebab-case
```

### constants 目录

此目录存放项目所有常量和枚举。

具体要求：

- 常量文件要以 `const` 为结尾，比如`login-const.js`、`file-const.js`
- 变量要：大写下划线，比如 `LOGIN_RESULT_ENUM`、`LOGIN_SUCCESS`、`LOGIN_FAIL`
- 如果是 枚举，变量必须以 `ENUM`为结尾，如：`LOGIN_RESULT_ENUM`、`CODE_FRONT_COMPONENT_ENUM`

目录结构：

```text
|constants
|-- index-const.js
|-- role-const.js
|-- employee-const.js
```

例子： employee-const.js

```js
export const EMPLOYEE_STATUS = {
  NORMAL: {
    value: 1,
    desc: '正常'
  },
  DISABLED: {
    value: 1,
    desc: '禁用'
  },
  DELETED: {
    value: 2,
    desc: '已删除'
  }
};

export const EMPLOYEE_ACCOUNT_TYPE = {
  QQ: {
    value: 1,
    desc: '短信登陆'
  },
  WECHAT: {
    value: 2,
    desc: '微信登录'
  },
  DINGDING: {
    value: 3,
    desc: '钉钉登录'
  },
  USERNAME: {
    value: 4,
    desc: '用户名密码登录'
  }
};

export default {
  EMPLOYEE_STATUS,
  EMPLOYEE_ACCOUNT_TYPE
};
```

### router 与 store 目录

这两个目录一定要将业务进行拆分，不能放到一个 js 文件里。

`router` 尽量按照 views 中的结构保持一致

`store` 按照业务进行拆分不同的 js 文件

### views 目录

目录要求，按照模块划分，其中具体文件名要求如下：

- 如果是列表页面，要以list为结尾，如 `role-list.vue`、`cache-list.vue`
- 如果是 表单页面，要以 form为结尾，如  `role-form.vue`、`notice-add-form.vue`
- 如果是 modal弹窗，要以 modal为结尾，如 表单弹窗 `role-form-modal.vue`，详情 `role-detail-modal.vue`
- 如果是 drawer 抽屉页面，要同上以 `Drawer`为结尾

```js
|-- views                                        视图目录
|   |-- role                                     role模块名
|   |   |-- role-list.vue                        role列表页面
|   |   |-- role-add-form.vue                    role新建页面
|   |   |-- role-update-form-modal.vue           role更新页面
|   |   |-- index.less                           role模块样式
|   |   |-- components                           role模块通用组件文件夹
|   |   |   |-- role-title-modal.vue             role弹出框组件
|   |-- employee                                 employee模块
|   |-- behavior-log                             行为日志log模块
|   |-- code-generator                           代码生成器模块
```
