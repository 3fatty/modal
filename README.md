# modal

> a global modal module for vue

## Build Setup

``` bash
# install dependencies
$ npm install # Or yarn install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout the [Nuxt.js docs](https://github.com/nuxt/nuxt.js).


全局的 dialog/modal 模块, 可在任意地方调用( 默认 context 为 window )
  主要优点:
    没有依赖, 静默初始化, 无需引用, 随处调用
    组件动态加载/卸载, 不会产生残留
    组件可叠加&共存, 彼此独立, 互不影响
    可自定义空间大, 样式/接口/数据格式/回调

  默认:
    样式:
      居中显示
      非穿透
      无阴影层
    事件绑定:
      .btn-close 按钮点击关闭弹窗
      .阴影层点击关闭弹窗
    预留钩子:
      beforeOpen
      afterOpen
      beforeClose
      afterClose

  方法调用:
    _modal.open(opt)        // 返回代表当前弹窗标识的 key ( String );
    _modal.close(key)       // 关闭指定的弹窗,

  opt 配置为:
    {
      // 默认保留参数
      <!-- sdf -->
      type        : '',           // * String, 要打开的弹窗窗口, 必选
      title       : '',           // String,   设置标题, 默认为'', 如果不传, 只显示关闭按钮
      fullScreen  : false,        // Boolean,  是否全屏, 默认为false ( 传 true 默认显示为半透阴影, 可自定义 )
      countdown   : 0,            // Number,   自动关闭窗口, 默认为 0, 单位为 s

      beforeOpen  : function,     // Function, hook for 生命周期, 支持 Promise 的同步处理
      afterOpen   : function,     // 同上
      beforeClose : function,     // 同上
      afterClose  : function      // 同上

      // 外部参数 ( 说明: )
      // 传入的所有 属性/方法 会挂载到打开的弹窗组件上, 可在组件内部直接用 this.xxx 访问到. 可用作回调
    }
