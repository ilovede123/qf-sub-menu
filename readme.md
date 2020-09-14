# 1.介绍

这是一个基于element-ui二次封装的一个侧边菜单栏组件,能快捷的根据数据,生成响应的子菜单,并且解决路径被激活的时候,菜单不激活的问题

# 2.安装

```
npm i qf-sub-menu -S
yarn add qf-sub-menu
```



# 3. 使用

### 1.定义数据源

首先需要有一组数据,基于权限生成的用户菜单路由配置数据,这些数据应该是这样的

```js
const routes = [
  {
    path: 'Welcome',
    name: 'Welcome',
    component: Welcome,
    meta: {
      name: '管理首页',
      icon: 'iconfont icon-shouye'
    }
  },
  {
    path: 'StudentManager',
    name: 'StudentManager',
    redirect:"/StudentManager/studentProduct",
    component: StudentManager,
    meta: {
      name: '学员管理',
      icon: 'iconfont icon-xueyuan'
    },
    children: [
      {
        path: 'studentProduct',
        name: 'studentProduct',
        component: studentProduct,
        meta: {
          name: '学员项目管理',
          icon: 'iconfont icon-wode1'
        }
      },
      {
        path: 'studentProfile',
        name: 'studentProfile',
        component: studentProfile,
        meta: {
          name: '学员资料',
          icon: 'iconfont icon-kaoqin2'
        }
      },
      {
        path: 'studentDormitory',
        name: 'studentDormitory',
        component: studentDormitory,
        meta: {
          name: '学员宿舍',
          icon: 'iconfont icon-shuju2'
        }
      }
    ]
  }
]

const router = new VueRouter({
  routes
})

export default router
```

如上所示,`meta`属性中,`name`属性最后会生成菜单名字,如果具有`children`属性,那么会对应生成子菜单,`icon`属性就是存放的菜单的图标类名(前提先安装字体图标)

### 2.修改main.js

引入qf-sub-menu组件,因为我们的组件基于element-ui二次封装,所以,我们要先配置element-ui

[element-ui](https://element.eleme.cn/#/zh-CN/component/quickstart)

安装element-ui

```
yarn add element-ui
```

```js
import Vue from "vue"
import App from "./App"
// 引入element-ui
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import subMenu from "qf-sub-menu" //引入我们的组件
Vue.use(ElementUI)
//使用Vue.use()将组件注入到所有的子组件
Vue.use(subMenu)

new Vue({
	h=>h(App)
}).$mount('#app')


```

### 3.嵌套在 element组件中的el-menu

像正常组件一样使用 <sub-menu>,并且需要注入数据源 属性名必须是 sideMenu(就是3-1所描述的数据源) 代码示例如下:

```js
<el-menu :default-active="$route.path"
                 class="el-menu-vertical-demo"
                 text-color="#4e5bf8"
                 ref="sideMenu"
                 active-text-color="#E47833"
                 :collapse="isCollapse">
    	//我们的组件
          <sub-menu :sideMenu='sideMenu'></sub-menu>
        </el-menu>
```

## 4.启用菜单激活样式

务必配合路由使用

直接在el-menu组件上加上属性 :default-active="$route.path"

```js
//:default-active="$route.path"


 <el-menu :default-active="$route.path" //加上此属性
                 class="el-menu-vertical-demo"
                 text-color="#4e5bf8"
                 ref="sideMenu"
                 active-text-color="#E47833"
                 :collapse="isCollapse">
          <!-- <subMenu :sideMenu='sideMenu'></subMenu> -->
          <sub-menu :sideMenu='sideMenu'></sub-menu>
        </el-menu>
```

