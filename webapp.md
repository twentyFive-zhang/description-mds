#   小程序埋点助手

##	功能用途

1. 小程序页面停留时间数据上报
2. 支持小程序点击事件数据上报

##	项目结构
```
.
├── build
│   └── index.js
├── doc
│   ├── README.md
│   └── example.jpeg
├── miniprogram
│   ├── app.js
│   ├── app.json
│   ├── pages
│   └── sitemap.json
├── package.json
├── plugin
│   ├── components
│   ├── index.js
│   ├── index.js.map
│   ├── pages
│   ├── plugin.json
│   └── utils
├── project.config.json
├── src
│   ├── @types
│   ├── utils.ts
│   └── index.ts
└── yarn.lock
```


### 引用 （app.json中加入） /taro中引入文件夹为 app.config.js

```
  "plugins": {
    "ewt-analysis-weapp": {
      "version": "dev",
      "provider": ""
    }
  }
```
### 初始化 （app.js最上面加入）

* 1.在原生小程序中引入
```
  const plugin = requirePlugin("ewt-analysis-weapp");
  const reWriteData = plugin.load({
    wx,
    getUid // 直接获取用户id  ()=>{return ''} 
  }); // 获取小程序的wx，用于使用api，原因:[小程序插件的api受限制]
  Page = plugin.reWritePage(Page, getCurrentPages); // 劫持Page生命周期，埋入function
  App = plugin.reWriteApp(App); // 劫持App生命周期，主要 onHide
```

* 2.在taro中引入
```
  // app.js中头部引入
  const plugin = Taro.requirePlugin("ewt-analysis-weapp");
  plugin.load({
    wx,
    getUid // 直接获取用户id  ()=>{return ''}
  }); // 获取小程序的wx，用于使用api，原因:[小程序插件的api受限制]
  Page = plugin.reWritePage(Page, Taro.getCurrentPages); // 劫持Page生命周期，埋入function
```

```
  // class App中引入 小程序休眠
  componentDidHide() {
    plugin._onAppHide.call(plugin);
  }
```

### 接口

```
  url: '',
  method: post,
  data: {
    sn: '', // 必须 string
    log: value, // 必须 string
  }
```

### log 详情

| 字段         | 类型   | 说明                                  | 通用类型| 默认值|
| ------------ | ------ | ------------------------------------- | ---- |---- |
| id           | string | 业务 Id                               | uuid |
| eventType    | string | 事件类型： page,click                 |
| browserEvent | string | 小程序页面事件类型：-load，-show,-hide,-unload |
| clickId      | string | 业务方传入                            |
| inout        | string | 进/出页面     in,out                         |
| inId         | string | 初始进入页面的id                             | uuid |
| uId          | string | 用户 id                               |
| userClient   | string | 客户端                                |  |weapp|
| jsVersion    | string | 当前版本                              |
| cookieId     | string | 游客 id                               |
| cookietime   | string | cookie 生成时间                       |
| platForm     | string | weapp平台                             |  ||
| model     | string | 手机型号                              |  ||
| useragent    | string | 代理标识                              | 暂无
| browserType  | number | 小程序类型                            |
| network      | string | 小程序网络类型                        | 
| refer        | string | 来源                                  |
| url          | string | 当前 url                              | 
| extra        | string | 扩展字段                              |
| focustime    | number | 聚焦时间                              | | 0
| duration     | number | 使用时长                              ||0
