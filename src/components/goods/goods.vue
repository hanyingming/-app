<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li class="menu-item" @click="selectMenu(index)" v-for="(item,index) in goods" :class="{'active': currentIndex === index}">
          <span class="icon" v-if="item.type>0" :class="classMap[item.type]"></span>
          <span class="text">{{item.name}}</span>
          <span class="good-count" v-show="item.count > 0">{{item.count}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li class="food-list food-list-hook" v-for="good in goods">
          <h1 class="title">{{good.name}}</h1>
          <ul>
              <li class="food-item" v-for="food in good.foods" @click="showFood(food)">
                <div class="icon">
                  <img :src="food.icon" :alt="food.name">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="description">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}份</span>
                    <span class="rating">好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cart-control-wrapper">
                    <cart-control  @addCart="addCart(food)" @decCart="decCart(food)" :food="food"></cart-control>
                  </div>
                </div>
              </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart :deliveryPrice="seller.deliveryPrice" :selectFoods="selectFoods" :minPrice="seller.minPrice"></shop-cart>
    <food :food="foodShow" ref="food"></food>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl"
  .goods
    position absolute
    display flex
    width 100%
    left 0
    top 3.48rem
    bottom 0.96rem
    overflow hidden
    .menu-wrapper
      flex 0 0 1.6rem
      width 1.6rem
      background #f3f5f7
      font-size 0
      font-weight 200
      .menu-item
        position relative
        z-index 5
        display flex
        align-items center
        box-sizing border-box
        width 100%
        height 1.08rem
        padding 0 0.24rem
        &.active
          z-index 10
          background #fff
          &::after
            display none
          .text
            font-weight 700
        &::after
          display block
          position absolute
          left 0.24rem
          bottom -1px
          width 1.12rem
          content ""
          border-top 1px solid rgba(7,17,27,0.1)
        &:last-child
          &::after
            display none
        .icon
          flex 0 0 0.24rem
          width 0.24rem
          height 0.24rem
          margin-right 0.08rem
          background-size cover
          background-repeat no-repeat
          &.decrease
            bg-img('decrease_3')
          &.discount
            bg-img('discount_3')
          &.guarantee
            bg-img('guarantee_3')
          &.invoice
            bg-img('invoice_3')
          &.special
            bg-img('special_3')
        .text
          font-size 0.24rem
          color rgb(24,20,20)
          line-height 0.28rem
        .good-count
          position absolute
          top 0.06rem
          right 0.06rem
          width 0.24rem
          height 0.24rem
          font-size 0.24rem
          line-height 0.24rem
          text-align center
          background-color red
          border-radius 2px
          color rgb(255,255,255)
    .foods-wrapper
      flex 1
      .food-list
        .title
          line-height 0.52rem
          font-size 0.24rem
          color rgb(147,153,159)
          height 0.52rem
          background #f3f5f7
          padding-left 0.28rem
          border-left-2px(#d9dde1)
        ul
          padding 0 0.36rem
        .food-item
          display flex
          padding 0.36rem 0
          border-bottom-1px(rgba(7,17,27,0.1))
          &:last-child
            border-none()
          .icon
            flex  0 0 1.16rem
            width 1.16rem
            margin-right 0.2rem
            & > img
              width 100%
              height 1.16rem
          .content
            position relative
            flex 1
            .name
              line-height 0.28rem
              font-size 0.28rem
              color rgb(7,17,27)
              margin-top 0.04rem
            .description
              line-height 0.2rem
              font-size 0.2rem
              color rgb(147,153,159)
              margin 0.16rem 0
            .extra
              line-height 0.2rem
              font-size 0
              color rgb(147,153,159)
              .count
                margin-right 0.24rem
                font-size 0.2rem
              .rating
                font-size 0.2rem
            .price
              font-size 0
              line-height 0.48rem
              font-weight normal
              color: rgb(147,153,159)
              .now
                font-size 0.24rem
                font-weight 700
                color: rgb(240,20,20)
                margin-right 0.16rem
              .old
                font-size 0.2rem
                text-decoration line-through
            .cart-control-wrapper
              position absolute
              right 0
              bottom 0
</style>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopCart from './../shopcart/shopcart.vue';
  import cartControl from './../cartcontrol/cartcontrol.vue';
  import Vue from 'vue';
  import food from './../food/food.vue';

  export default {
    data () {
      return {
        goods: [],
        url: {
          goods: '/api/goods',
          seller: '/api/seller'
        },
        classMap: ['decrease', 'discount', 'guarantee', 'invoice', 'special'],
        menuScroll: null,
        foodsScroll: null,
        listHight: [], // 存储右侧各类商品距离顶部的高度数组
        scrollY: 0, // 实时记录右侧滚动条的滚动的距离
        seller: {},
        foodShow: {} // 商品详情的对象
      };
    },
    components: {
      shopCart,
      cartControl,
      food
    },
    computed: {
      currentIndex () { // 记录当前选中的左侧商品类别的下标
        for (let i = 0; i < this.listHight.length - 1; i++) {
          let height1 = this.listHight[i];
          let height2 = this.listHight[i + 1];
          if ((this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods () {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    mounted: function () { // 获取商家与商品信息数据
      this.getSellerData();
      this.getGoods();
    },
    methods: {
      getSellerData () { // 获取商家信息数据
        this.$http.get(this.url.seller).then((res) => {
          this.seller = res.data.result;
        }).catch((err) => {
          console.log(err);
        });
      },
      getGoods () { // 获取商品信息数据
        this.$http.get(this.url.goods).then((res) => {
          let resData = res.data;
          if (resData && resData.status === 0) {
            this.goods = resData.result;
            this.$nextTick(() => { // vue的$nextTick()方法保证dom节点已经存在
              this._initScroll(); // 添加滚动条
            });
          }
        }).catch((err) => {
          console.log(err);
        });
      },
      _initScroll () { // 初始化better-scroll组件
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {click: true});
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {probeType: 3, click: true});
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y)) + 1;
        });
        this._calculateHeight();
      },
      _calculateHeight () { // 计算右侧各种商品类别的高度并添加到listHeight数组中
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHight.push(height);
        }
      },
      selectMenu (index) { // 点击选中左侧商品分类
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        this.foodsScroll.scrollToElement(foodList[index], 300);
      },
      addCart (food) { // 添加商品购买数量
        if (food.count) { // 购买数量自增
          food.count += 1;
        } else { // 首次购买
          Vue.set(food, 'count', 1);
        }
      },
      decCart (food) { // 减少商品购买数量
        if (food.count) { // 购买数量自减1
          food.count -= 1;
        } else { // 首次操作,可删除
          Vue.set(food, 'count', 0);
        }
      },
      showFood (food) {
        this.foodShow = food;
        this.$refs.food.showFood();
      }
    }
  };
</script>
