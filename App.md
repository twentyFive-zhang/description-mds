app 动态化



1.是否新立项目，减少耦合度，为以后扩展性做准备（不只是动态app的xml，可以提供多种模版，生成代码字符串或文件）

2.所有数据在失焦的时候去重新渲染视图，减少性能消耗

3.h5兼容性（如果新立项目，可为多页面打包，类似专题，实际上关键代码都是通用的）

4.每个组件无论在哪一步都需要支持选中至元素或元素集合的功能，即元素树，类dom树，并有选中效果和选中后的数据集合，提供编辑功能

5.每个抽象组件提供默认样式集合和数据集合，并在具象组件中提供修改功能，（具象组件修改样式后是否另存为？同理页面编辑？  或在编辑完后后台给予当前具象组件应用的页面合集，由用户确认是否在原具象组件更改，即一改所有页面再次编辑的时候都会相应更改）

6.因为页面保存时，即生成xml数组，所以到下次页面保存时，xml数据不会发生改变，页面中的具象组件修改后，对于页面下次保存之前无效

7.具象组件版本号 

8.选择组件后，一些数据源列表
9.vue 或者 react vue3的data支持比较舒服，数据配置会相对react简单 公司需求使用react
10.服务器端渲染？ 使用next，nuxt暂时不稳定，最新的坑暂不见底，旧版的手感不如vue3




## 底层设置铺设

### 基础部分

#### 1.基础通用属性部分

1. 基础属性转换
   ​	相同部分属性整理 驼峰转中划线 用css引用，class命名 驼峰
   ​	不同部分属性整理 根据app 生成css class命名(也用驼峰) /引入css 文件

2. 组合（如滑动/走马灯等css无法直接生成）属性转换
   ​	根据ui框架呢还是自己写组件？ 自己写的话需要先全局思考 ------- 使用 ant-mobile ， react的h5ui有点少

3. app flex 布局 （跟h5不同，除flex属性，其他均为元素自有属性，大都相同，均用驼峰）

#### 2.基础通用元素部分

1. 真正元组件（对于三端）

  * Label:  对应 span 最小单元文字 /
    *  值 text
  * View： 对应div 
  * ImageView： 对应img 最小单元图片 /
    * 值 image
  * Container：容器组件
  * ScrollView： 带滚动条的Container 
    * 动画效果
      * bounces 边框拉取反弹动画   bool    默认YES   //？ h5大概率不支持

    * 事件
      * pagingEnabled  ？滑动加载  如果false

    * 滚动条效果
      * showsVerticalScrollIndicator 是否显示纵向滑动条  bool     默认YES
      * showsHorizontalScrollIndicator 是否显示横向滑动条 bool    默认YES
  * TableView： 只能竖放的一行一个TableViewCell/竖着的row  本质Container
   * TableViewCell:  父元素必须是TableView
  * CollectionView： 带flex布局的类似row/col 本质Container
  * CollectionViewCell：父元素必须是CollectionView
  * PageControl：Page xml的最外层 子值可以是【CollectionView/TableView 等】 本质最大Container
  * TouchView：带点击事件的Container 
    * 点击事件 onTouch 
      * 路由事件 {data.route}

    * 埋点事件 onMose
      * 曝光事件
      * 点击事件 ？ 和onTouch不一样吗？？？
  * TabViews：
  * InfiniteScrollView： 走马灯Container/ 和ScrollView 区别？
    * 值 autoLoop：boolean  /   duration：每次动画的时间间隔

### 组件部分

#### 抽象组件 参照 AbstractComponentItem

1. 组件构成  基础为json
   * UI （本质 UI TREE： 由上述元素组成）
     * 提供元素的排列组合和增删改，支持到最小元素的选中效果，和相应选中的基础属性的配置 AbstractComponentItem.
    * 

#### 具象组件(抽象组件 & 数据源 & 业务相关关联 ) 参照ConcreteComponentItem

1. 组件构成  基础为json

  * UI （本质 UI TREE： 由上述元素组成） 继承自 抽象组件，在当前层面不支持修改 ？
  * 数据绑定 列表
  * 埋点数据 方法如数据绑定


### 页面部分 参照 PageJson const {list,...rest}=pageJson

#### 基础设置  rest 的参数

#### 组件列表 list 数据 参照。PageItem

1. UI（本质 UI TREE）
   * 支持到最小元素的选中效果，和相应选中的基础属性的配置修改 PageItem.childrenSetting
2. 数据源选择 PageItem.dataSourceData
3. 埋点选择 PageItem.moseData








具体数据流走向 AbstractComponentItem  ---》 ConcreteComponentItem ---》 PageItem ---》 PageJson


## 后台部分

### 总体页面

#### 数据源管理？ 是否需要



#### 埋点列表管理？是否需要

#### 抽象组件管理

1. 列表页面（新增/编辑/？删除？/上线/下线 按钮）
   * 功能 按钮 新增/编辑/？删除？/上线/下线
   * 接口 del？/changeStatus /getList
2. 新增/编辑页面 （）
   * 功能 新增/编辑
   * 接口 detail/add/update

#### 具象组件管理

1. 列表页面（新增/编辑/？删除？/上线/下线 按钮）

   * 功能 按钮 新增/编辑/？删除？/上线/下线
   * 接口 del？/changeStatus /

2. 编辑页面

   

   * 功能 新增/编辑 /数据源添加/埋点数据添加
   * 接口 detail/add/update /（数据源列表 getDataSourceList / 埋点列表 getMoseList ？绑定在具象组件管理？）

#### Page页面管理

1. 列表页面（新增/编辑/删除/上线/下线 按钮）
   * 功能 按钮 新增/编辑/？删除？/上线/下线
   * 接口 del？/changeStatus /
2. 编辑页面
   * 功能 新增/编辑
   * 接口 detail/add/update /（数据源列表 getDataSourceList / 埋点列表 getMoseList ？绑定在具象组件管理？） 抽象组件detail / 具象组件detail / 
   * 保存时生成xml 和 h5 页面生成的字符串 另存为一个接口，id与当前页面id相同

### 重头戏

#### 编辑部分

1. 表单 每个单独的Item 都会有相应的formJson，去编辑相应数据

2. 预览部分 与h5 生成所用组件相同， 支持到最小元素的选中和相应配置的数据反显？（比较麻烦）

3. 数据流自上而下 使用context/provider 管理最外表单数据

4. util 工具根据pageJson 生成相应的xml/h5 字符串 （递归 耗时会很久）/包括保存时xml/h5生成和单个PageItem 的xml 预览

5. h5 的事件绑定和渲染 （最好选择是服务器渲染 ，保存编辑时，生成一个ejs 文件，直接服务器渲染，性能损耗相对小，？需要钻研下！）

6. 数据源添加时的数据可视化 参照 outDataItem



   ​    
