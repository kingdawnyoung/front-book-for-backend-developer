# element-ui 

来自饿了么的一套完整的前端组件库

## 安装

和使用vue一样，直接通过链接引入样式和脚本，或者通过npm安装配合webpack打包使用

通过样式脚本引入
```html
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
```

通过npm安装

```bash
npm i element-ui -S
```

引入全部element的组件

```js
import Vue from 'vue'
import Element from 'element-ui'
Vue.use(Element, { size: 'small' })
//引入element-ui的theme-chalk主题
import 'element-ui/lib/theme-chalk/index.css'

```

接下来我们就可以在页面中愉快的使用element-ui组件啦

## 组件

element提供的组件很多，总体分为基础组件、表单组件、数据展示类组件、通知类组件、导航类组件、其他组件

### 基础组件

#### 布局

element-ui使用24分栏的栅格式布局`<el-row>`表示行，`<el-col>`表示列

通过`<el-col>`的`span`可以指定列占据的栅格数量；`offset`指定栅格左侧的间隔的栅格数量；`push`和`pull`分别标识栅格向右和左移动个数

#### 布局容器

element-ui为我们提供了布局容器，方便我们搭建快速搭建页面的基础结构

`<el-container>`：外层容器。

`<el-header>`：顶栏容器。

`<el-aside>`：侧边栏容器。

`<el-main>`：主要区域容器。

`<el-footer>`：底栏容器。

#### 图标

一套常用的图标集合，使用时设置类名`el-icon-iconName`即可。

#### 按钮

按钮是最为常用的组件，element-ui的按钮组件为`<el-button>`。按钮也提供了一系列属性控制按钮的尺寸(size)、类型(type)、图标(icon)、状态(loading, disabled)等等

### 表单组件