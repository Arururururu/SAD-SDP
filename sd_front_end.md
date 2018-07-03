

### 1、技术选型以及理由

- 微信小程序框架

   我们选择了微信小程序开发框架作为用户点餐系统的前端框架。小程序框架提供了自己的视图层描述语言 WXML 和 WXSS，
   以及基于 JavaScript 的逻辑层框架，容易上手，并在视图层与逻辑层间提供了数据传输和事件系统，支持响应式、双向数据绑定等特性，
   可以让数据与视图非常简单地保持同步：当做数据修改的时候，只需要在逻辑层修改数据，视图层就会做相应的更新，
  因而能够很好地支持丰富的用户交互体验支持，可以让开发者方便的聚焦于数据与逻辑上；
  框架还管理了整个小程序的页面路由，可以做到页面间的无缝切换，并给以页面完整的生命周期，
  开发者需要做的只是将页面的数据，方法，生命周期函数注册进框架中，其他的一切复杂的操作都交由框架处理；
  此外，框架还提供了微信风格的组件和丰富的api，实现了尽量以简单、高效的方式让开发者可以在微信中开发具有原生 APP 体验的服务。
  且4g网络和移动支付的普及推动了微信小程序作为点餐系统的应用发展，
  使用微信小程序作为点餐系统对用户更加快捷和便利，因此选择微信小程序作为前端框架对用户和开发者都十分友好。


### 2、架构设计

- MINA框架
   
   微信团队为小程序提供的框架命名为MINA应用框架，MINA框架让开发者能够非常方便地使用微信客户端提供的各种基础功能与能力，快速构建一个应用。
   
   在页面视图层，MINA的wxml是一套类似html标签的语言以及一系列基础组件。开发者使用wxml文件来搭建页面的基础视图结构，使用wxss文件来控制页面的展现样式。AppService应用逻辑层是MINA的服务中心，由微信客户端页面视图层外启用异步线程单独加载运行，MINA 中所有使用 javascript 编写的交互逻辑、网络请求、数据处理都必须在 AppService 中实现，但AppService中不能使用DOM操作相关的脚本代码。小程序中的各个页面可以通过AppService实现数据管理、网络通信、应用生命周期管理和页面路由管理。
   
　 MINA框架还为页面组件提供了bindtap、bindtouchstart等事件监听相关的属性，来与AppService中的事件处理函数绑定在一起，实现面向AppService层同步用户交互数据。MINA框架的核心是一个响应的数据绑定系统，提供了很多方法将AppService中的数据与页面进行单向绑定，让数据与视图非常简单地保持同步，当AppService中的数据变更时，会主动触发对应页面组件的重新渲染。MINA使用virtualdom技术，加快了页面的渲染效率。
  
   MINA程序包含一个描述整体程序的app和多个描述各自页面的page，一个MINA程序主体部分由三个文件组成，必须放在项目的根目录，如下：
  
   文件       |   作用    |
  -------     |-------    |
  app.js      |  小程序逻辑 |
  app.json    | 小程序公共设置|
  app.wxss    | 小程序公共样式表|

   一个MINA页面由四个文件组成，分别是：
  
  文件      |   作用    |
  -------     |-------    |
  js          |  页面逻辑 |
  json        | 页面配置  |
  wxss        | 页面样式表|
  wxml        | 页面结构 |
  
  MINA框架图如下：
  
  ![小程序框架图](http://wx3.sinaimg.cn/mw690/85eb32d8gy1fsx1xhq7dhj20n30irmxr.jpg)


### 3、模块划分

```txt
├── project.config.json //开发工具配置
├── app.js //小程序的全局逻辑文件
├── app.json //小程序的全局配置
├── app.wxss //小程序的全局样式
├── images //小程序利用到的图片
|
├── pages //小程序的页面文件存放文件夹
|   ├── authorize // 授权登录页面
│   │   ├── authorize.js //授权登录页面的逻辑文件
|   |   ├── authorize.json //授权登录页面配置文件
│   │   ├── authorize.wxml // 授权登录页面的结构文件
│   │   └── authorize.wxss  // 授权登录页面的样式文件
|   |
│   ├── index //主菜单页面
│   │   ├── index.js //主菜单页面的逻辑文件
|   |   ├── index.json //主菜单页面配置文件
│   │   ├── index.wxml // 主菜单页面的结构文件
│   │   └── index.wxss  // 主菜单页面的样式文件
|   |
│   ├── recommendation //今日推荐页面
│   |   ├── recommendation.js //今日推荐页面的逻辑文件
│   |   ├── recommendation.json //今日推荐页面配置文件
│   |   ├── recommendation.wxml //今日推荐页面的结构文件
│   |   └── recommendation.wxss //今日推荐页面的样式文件
|   |
│   ├── recommendation-details //今日推荐卡片细节页面
│   |   ├── recommendation-details.js //今日推荐卡片细节页面的逻辑文件
│   |   ├── recommendation-details.json //今日推荐卡片细节页面配置文件
│   |   ├── recommendation-details.wxml //今日推荐卡片细节页面的结构文件
│   |   └── recommendation-details.wxss //今日推荐卡片细节页面的样式文件
|   |
│   ├── cart //购物车页面
│   |   ├── cart.js //购物车页面的逻辑文件
│   |   ├── cart.json //购物车页面配置文件
│   |   ├── cart.wxml //购物车页面的结构文件
│   └── └── cart.wxss //购物车页面的样式文件
|
├── component //自定义组件
│    ├── card //今日推荐卡片组件
│    |   ├── card.js //今日推荐卡片组件的逻辑文件
│    |   ├── card.json //今日推荐卡片组件的配置文件
│    |   ├── card.wxml //今日推荐卡片组件的结构文件
│    |   └── card.wxss //今日推荐卡片组件的样式文件
|    |
|    ├── hSwiper //滑动框组件
│    |   ├── hSwiper.js //滑动框组件的逻辑文件
│    |   ├── hSwiper.json //滑动框组件的配置文件
│    |   ├── hSwiper.wxml //滑动框组件的结构文件
│    |   └── hSwiper.wxss //今日推荐卡片组件的样式文件
|    |
|    └── orderItem //购物车or订单item组件
│    |   ├── orderItem.js //订单item的逻辑文件
│    |   ├── orderItem.json //订单item的配置文件
│    |   ├── orderItem.wxml //订单item组件的结构文件
│    └── └── orderItem.wxss //订单item组件的样式文件
|
└── utils //公共的js代码
     └── util.js

```

### 4、列出软件设计技术

### 5、给出具体设计在源代码中的位置，指明对应的模块和代码
