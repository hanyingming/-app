# Vue2.0 点餐app模块

## 安装运行

# 项目使用的开发工具：
    webstorm

# 项目中使用到的技术与方法:
    svg图片制作为字体图标，并引入此字体文件。
    mock后台数据
    stylus：css预处理器
    eslint：代码静态检查
    reset.css ：初始化代码样式
    webpack ：项目模块构建工具
    vue-cli : 构建webpack项目 vue init webpack eapp
    Vue2.0 + Vue-router + vue-resource

# 文件结构说明：
    .editorconfig : 编辑器的配置
    .eslintignore : eslint代码检查忽略的目录
    .eslintrc.js  : eslint代码检查的配置
        .eslintrc.js文件添加了： 'semi': ["error", "always"]  //用于代码中需要强中添加分号
    .gitignore    : 向github上传文件时忽略的文件
    .postcssrc.js : postcss模块用于处理浏览器的css代码的兼容，该文件
    data.json     : mock的项目数据
    index.html    : SPA项目的单页面的基本结构文件
    package.json  : 项目的相关依赖配置
    README.md     : 项目的说明文件

# 项目目录说明：
      build        :  webpack构建项目的编译时的基础配置目录
      config       :  wepack构建项目的项目配置文件目录
      node_modules :  项目依赖模块文件目录
      rosource     :  项目资源文件目录
      src          :  项目源码文件目录
        common     :  项目通用文件目录
          fonts    :  字体文件目录
          js       :  js文件目录
          stylus   :  styl文件目录
        components :  项目组件目录
        router     :  项目路由配置文件目录
      static       :  项目静态文件目录
           css     :  css静态文件目录（存放reset.css等）


# 项目开发中遇到的异常：
    ① 异常：
       * !!vue-style-loader!css-loader?{"minimize":false,"sourceMap":false}!../../node_modules/vue-loader/lib/style-compiler/index?{"vue":true,"id":"data-v-1d57e5ea","scoped":false,"hasInlineConfig":false}!stylus-loader?{"sourceMap":false}!../../node_modules/vue-loader/lib/selector?type=styles&index=0!./a.vue in ./src/components/a.vue
       To install it, you can run: npm install --save !!vue-style-loader!css-loader?{"minimize":false,"sourceMap":false}!../../node_modules/vue-loader/lib/style-compiler/index?{"vue":true,"id":"data-v-1d57e5ea","scoped":false,"hasInlineConfig":false}!stylus-loader?{"sourceMap":false}!../../node_modules/vue-loader/lib/selector?type=styles&index=0!./a.vue
       解决方式：
          npm install stylus stylus-loader --save-dev

# 项目开发步骤：
     1.需求分析
          分析了解项目的需要开发的功能模块。
     2.资源准备
          从UI设计师手中获取页面设计以及相应的图标设计资源。
     3.图标字体制作
          单一色彩的图标可以制作为字体文件。
          该项目中奖resource文件中的svg目录的矢量图标上传到iconMooon网站制作为字体文件。
          制作完字体文件后，从网站上下载下来。
     4.项目目录设计
          src 目录中添加
             common :  项目通用文件目录
                 fonts :  字体文件目录
                 js    :  js文件目录
                 stylus:  styl文件目录
          static 目录中添加：
              css目录：css静态文件目录（存放reset.css等）

          将从网站上下载下来的字体文件存放到 src/common/fonts目录内，
          style.css样式文件存放到src/common/stylus目录内并重命名为icon.styl
     5.mock数据
          data.json  : mock的项目数据
          在build/dev-server.js模拟后台接口api，通过接口访问可以获取data.json中的数据。
     6.index.html文件
          设置viewport;引入reset.css文件（重置样式）;
          引入rem布局的js代码
     7.组件规划
        header：头部组件
        subNav: 模块导航组件
        goods:展示商品内容的组件
        seller:商家内容组件
        ratings:评价内容组件
        star: 星星评分组件
      8.设计规划路由
        /goods  商品页
        /seller 商家页
        /ratings 评价页
      9.组件开发：
           ① header（头部组件的开发）：
                模块化布局：
                    内容区域：使用flex布局划分为左右两个区域。
                    公告栏区域：使用flex布局划分为左中右区域，中间区域自适应。
                    详细页区域：首先进行css stictiy footers布局；
                         星星评分区域使用star.vue星星评分组件：此组件接收父组件的两个参数：size(星星的尺寸)，score(评分)；
                         其它区域主要使用flex布局。
                    特效：
                        点击优惠个数打开详细页区域（添加了过渡效果）。
                        header区域使用了滤镜效果。

            ② subNav: 模块导航组件
                  该组件主要是进行路由跳转配置。

            ③ goods:展示商品内容的组件。
                  模块化布局：
                     思路：首先分成左右两部分区域，使用flex布局左固定右自适应，右边区域：再次使用flex布局左固定右自适应。
                     主要区域划分：
                       食物分类列表区域。
                       食物详细列表展示区域。
                     使用的第三方组件：better-scroll
                     实现的效果：
                        右侧食物详细列表展示区域滚动时，左侧自动选中相应的食物类别。
                        点击左侧食物类别时，右侧自动滚动到相应的列表的食物列表区域。

             ④ cartcontrol:购物控制组件
                  模块化布局：
                      设计思路：
                        主要展示三部分内容：增加商品图标、减少商品图标、商品数量；起始只有购买商品图标，
                        点击购买商品图标，商品数量信息和减少商品图标显示出来。
                        当点击商品购买图标时，为该商品对象添加一个count属性：记录该商品被购买的数量。
                        使用vue的实时计算来统计购买商品的总数量和购买商品的总价格并实时反应在购物车组件上。
                      实现效果：
                        点击添加商品图标，使用过渡的动画效果显示完整的购物控制组件。
                        能够购买商品，并将商品的数量与总价反应到购物车上

             ⑤ shopcart:购物车组件：
                  模块化布局：
                    设计思路：使用fixed布局并设置z-index使其定位在页面的底部。
                          展示内容分为底部购物车信息部分和购买商品详细列表。
                          购物车信息部分依据UI设计图分为左右两部分，左自适应右固定，使用flex布局。
                          当向购物车中添加了商品，购物车区域显示购买商品的数量和商品总价。
                          根据购买商品的数量动态设计付款描述；
                          根据购买商品的总价设计样式；
                          实时计算是否显示购物车商品列表，当购买商品的数量大于0时，点击购物车可以切换展示购物商品信息列表；
                          在购物车购买商品信息列表中，用户可以继续购买此商品，也可以清空购物车。
                          购物车商品信息列表设置有最大高度，并使用better-scroll组件支持数据滚动浏览。
                     使用的第三方组件：better-scroll
                     实现的效果：
                         实时显示购买商品的数量和商品总价；
                         根据购买商品的数量动态显示付款描述；
                         当购买商品的数量大于0时，点击购物车可以切换展示购物商品信息列表；
                         购物车商品信息列表中商品数量的增减和清空购物车。

             ⑥商品详情信息Food组件：
                  设计思路：
                    点击商品以向左滚动的动画效果展示商品详情，在商品详情页面用户可以购买当前商品，
                    如果进入该页面之前用户已经向购物车中添加了改商品，页面展示购买商品控制组件，否则显示加入购物车按钮；
                    当点击购物车按钮，过渡切换到购物控制组件按钮。
                    用户可以切换展示商品评价信息列表。
                  实现效果：
                     可以将当前商品添加到购物车。
                     用户可以切换展示商品评价信息列表。
# 开发心得：
    (一)Vue父子组件通信：
        ①父子传值props
            子组件不能修改props传递的属性值。
            只能通过自定义事件，通过父组件修改子组件的值。
        ②父子传递对象props
            子组件中允许直接修改传递对象的值并能够反映到父组件上。
            也可以通过自定义事件，通过父组件修改子组件的值。
            但是：如果此对象需要添加新的属性时，必须使用
              import Vue from 'vue';Vue.set(对象名，'属性名’,属性初始值);才能将添加的对象属性反映到组件视图上。

    (二)等宽等高布局：
         .avatar-wrapper
            position: relative;
            width: 100%;
            height: 0;
            padding-top:100%;
            .avatar
              position: absolute
              left: 0
              top: 0
              width: 100%;
              height: 100%;
        注释：
          padding-top:100%或者padding-bottom:100%是依据width来进行计算的

    (三)1px的处理
      border-bottom-1px($color)
        position relative
        &:after
          display block
          position absolute
          left 0
          bottom 0
          width 100%
          content ""
          border-top 1px solid $color

      @media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5)
        .border-1px
          &::after
            -webkit-transform: scaleY(0.7);
            transform scaleY(0.7)

      @media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2)
        .border-1px
          &::after
            -webkit-transform: scaleY(0.5);
            transform scaleY(0.5)

    (四)sticky footers布局(页面内容长度不够时，页脚块在窗口的底部，如果内容足够长，页脚块被内容向下推送。)
      <div>
        <div class='wrapper clearfix'>
            <!-- min-height:100% -->
            <div class='main'>
              <!-- 填充内容  padding-bottom一定的距离-->
            </div>
          </div>
          <div class='close'>
             <!-- 页脚块展示的内容 -->
          </div>
      </div>

      <style>
        .wrapper
          width 100%
          min-heigh 100%
          .main
            padding-bottom 120px
        .close
          margin-top -120px
      </style>
