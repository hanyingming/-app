<template>
  <div class="shop-cart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper" @click.stop="toggleList">
          <div class="logo" :class="{active:totalCount>0}">
            <i class="icon-shopping_cart"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{active:totalCount>0}">{{totalPrice}}</div>
        <div class="description">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">{{payDesc}}</div>
      </div>
    </div>
    <transition name="fade">
      <div class="shop-cart-list border-1px" v-show="listShow">
        <div class="header">
          <h1 class="title">购物车</h1>
          <span class="emptyCart" @click="emptyCart">清空</span>
        </div>
        <div class="content-list" ref="contentList">
          <ul>
            <li class="food-item" v-for="food in selectFoods">
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span class="sign">￥</span>
                <span class="num">{{food.price*food.count}}</span>
              </div>
              <div class="cart-control-wrapper">
                <cart-control  @addCart="addCart(food)" @decCart="decCart(food)" :food="food"></cart-control>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl";

  .shop-cart
    position fixed
    left 0
    bottom 0
    width 100%
    height 0.96rem
    z-index 50
    .content
      display flex
      background #141d27
      width 100%
      .content-left
        flex 1
        font-size 0
        .logo-wrapper
          display inline-block
          position relative
          top -0.2rem
          width 1.12rem
          height 1.12rem
          border-radius 50%
          margin 0 0.24rem
          background #141d27
          .logo
            width 0.88rem
            height 0.88rem
            line-height 0.88rem
            margin-top 0.12rem
            margin-left 0.12rem
            border-radius 50%
            background-color rgba(255,255,255,0.2)
            text-align center
            &.active
              background-color rgb(0,160,220)
              i
                color rgb(255,255,255)
            i
              font-size 0.48rem
              vertical-align middle
              color rgba(255,255,255,0.4)
          .num
            position absolute
            top 0
            left: 0.64rem
            width 0.48rem
            height 0.32rem
            line-height 0.32rem
            text-align center
            font-size 0.24rem
            font-weight 700
            border-radius 0.12rem
            color rgb(255,255,255)
            background-color rgb(240,20,20)
        .price
          display inline-block
          font-size 0.32rem
          line-height 0.48rem
          color rgba(255,255,255,0.4)
          font-weight 700
          padding 0.08rem 0.24rem 0.08rem 0
          border-right 1px solid #cccccc
          margin-right 0.24rem
          &.active
            color rgb(255,255,255)
        .description
          display inline-block
          font-size 0.32rem
          line-height 0.48rem
          color rgba(255,255,255,0.4)
      .content-right
        flex 0 0 2.1rem
        width 2.1rem
        .pay
          background-color  rgba(255,255,255,0.2)
          line-height 0.96rem
          font-weight 700
          font-size 0.24rem
          text-align center
          &.not-enough
            color rgba(255,255,255,0.4)
          &.enough
            background-color #00b43C
            color rgb(255,255,255)
    .shop-cart-list
      position absolute
      left 0
      top 0
      width 100%
      z-index -1
      transform translate3d(0,-100%,0)
      background-color #fff
      &.fade-enter-active, &.fade-leave-active
        transition all 0.4s
      &.fade-enter, &.fade-leave-active
        transform translate3d(0,0,0)
      .header
        display flex
        justify-content space-between
        height 0.8rem
        line-height 0.8rem
        padding 0 0.36rem
        background-color #f3f5f7
        border-bottom-1px(rgba(7,17,27,0.1))
        .title
          font-size 0.28rem
          color rgb(7,17,27)
          font-weight 200
        .emptyCart
          font-size 0.24rem
          color rgb(0,160,220)
      .content-list
        max-height 4.2rem
        overflow hidden
        padding 0 0.36rem
        .food-item
          display flex
          justify-content space-between
          align-items center
          height 0.96rem
          border-bottom-1px(rgba(7,17,27,0.1))
          &:last-child
            border-none()
          .name
            font-size 0.28rem
            line-height 0.48rem
            color rgb(7,17,27)
            font-weight 700
          .price
            flex 1
            text-align right
            padding-left 0.36rem
            line-height 0.48rem
            color rgb(240,20,20)
            .sign
              font-size 0.2rem
              font-weight normal
            .num
              font-size 0.28rem
              font-weight 700
          .cart-control-wrapper
            padding-left 0.24rem

</style>
<script type="text/ecmascript-6">
  import cartControl from './../cartcontrol/cartcontrol.vue';
  import BScroll from 'better-scroll';
  export default {
    props: {
      selectFoods: {
        type: Array,
        default () {
          return [];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    computed: {
      totalPrice () { // 实时计算购买商品的总价
        let totalPrice = 0;
        this.selectFoods.forEach((food) => {
          totalPrice = totalPrice + food.price * food.count;
        });
        return totalPrice;
      },
      totalCount () { // 实时计算购买的商品数量
        let totalCount = 0;
        this.selectFoods.forEach((food) => {
          totalCount = totalCount + food.count;
        });
        return totalCount;
      },
      payDesc () { // 根据购买商品的数量动态设计付款描述
        if (this.totalCount === 0) {
          return '￥' + this.minPrice + '元起送';
        } else if (this.totalPrice < this.minPrice) {
          let diff = (this.minPrice * 10 - this.totalPrice * 10) / 10;
          return `还需￥${diff}元起送`;
        } else {
          return '去结算';
        }
      },
      payClass () { // 根据购买商品的总价设计样式
        if (this.totalPrice === 0 || this.totalPrice < this.minPrice) {
          return `not-enough`;
        } else {
          return 'enough';
        }
      },
      listShow () { // 实时计算是否显示购物车商品列表
        if (!this.totalCount) {
          this.fold = true;
          return false;
        }
        let show = !this.fold;
        if (show) {
          this.$nextTick(() => {
            if (!this.contentList) {
              this.contentList = new BScroll(this.$refs.contentList, {click: true});
            } else {
              this.contentList.refresh();
            }
          });
        }
        return show;
      }
    },
    components: {
      cartControl
    },
    mounted: function () {
    },
    data () {
      return {
        fold: true // 默认购物车详细信息列表折叠
      };
    },
    methods: {
      addCart (food) { // 添加商品购买数量
        if (food.count) { // 购买数量自增
          food.count += 1;
        }
      },
      decCart (food) { // 减少商品购买数量
        if (food.count) { // 购买数量自减1
          food.count -= 1;
        }
      },
      toggleList () { // 切换显示购物车列表
        if (!this.totalCount) {
          return;
        }
        this.fold = !this.fold;
      },
      emptyCart () { // 清空购物车
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      }
    }
  };
</script>
