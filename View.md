# View

View层负责与用户直接打交道：渲染页面、接受用户的操作输入，侧重于展示型交互性逻辑。

## 视图组件
1. 非标准组件
```
// 匿名组件
export default () => {
  return <div>hello world</div>;
}

// 命名组件
const Picture = (props) => {
  return (
    <div>
      <img src={props.src} />
      {props.children}
    </div>
  )
}
```

2. 标准组件

组件必须继承React.Component这个基类，然后必须有一个render方法，给出组件的输出。

```
import React from 'react';

class ShoppingList extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null
    };
  }
  
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

export default ShoppingList;
```

> 注：局部简单组件可以用非标准形式，主控件必须用标准形式。

## 页面布局

**Grid布局**

Grid（栅格）布局一般在页内布局使用，其特点就是按照一定比例划分页面，能够随着屏幕的变化依旧保持比例，可实现页面的弹性伸缩。

**Layout布局**

Layout则一般用于整体框架级别的布局设计，它抽象了大部分框架布局结构，使得只需要填空就可以开发规范专业的页面整体布局。

## 路由和菜单

脚手架通过结合一些配置文件、基本算法及工具函数，搭建好了路由和菜单的基本框架，主要涉及以下几个模块/功能：

+ 路由管理 通过约定的语法根据在 router.config.js 中配置路由。
+ 菜单生成 根据路由配置来生成菜单。菜单项名称，嵌套路径与路由高度耦合。
+ 面包屑 组件 PageHeader 中内置的面包屑也可由脚手架提供的配置信息自动生成。

1. 路由

目前所有的路由都通过router.config.js来统一管理，在umi的配置中增加了一些参数，如name，icon，hideChildrenInMenu，authority，来辅助生成菜单。

+ name 和 icon分别代表生成菜单项的文本和图标。
+ hideChildrenInMenu 用于隐藏不需要在菜单中展示的子路由。
+ hideInMenu 可以在菜单中不展示这个路由，包括子路由。
+ authority 用来配置这个路由的权限，如果配置了将会验证当前用户的权限，并决定是否展示。

> 注：name可能和实际展示的菜单名不同，原因是试用了i18n。

2. 菜单

菜单根据router.config.js文件生成，具体逻辑在src/models/menu.js中的formatter方法实现。

> 如果你的项目并不需要菜单，你可以直接在 BasicLayout 中删除 SiderMenu 组件的挂载。并在 src/layouts/BasicLayout 中 设置 const MenuData = []。
> 如果你需要从服务器请求菜单，可以将 menuData 设置为 state，然后通过网络获取动态修改 state。

3. 面包屑

面包屑由 PageHeaderWrapper 实现，只要在页面中引入PageHeaderWrapper组件即可。

## 最佳实践

> 一个页面通常由多个不同的组件构成，以常用的列表页面为例，它往往包含一个表格和多个弹出窗口，此时应将包含表格在内的主体部分当做一个父组件，而把
> 弹出窗口和其它更小的部分看做不同的子组件。子组件通过props属性共享父组件的方法和属性。这样只需要在父组件connect注入model即可，子组件则不再
> 需要关注模型对象。