<template>
  <transition name="move">
    <div class="food" v-show="showFlag" ref="food">
      <div class="food-content">
        <div class="img-header">
          <img :src="food.image" :alt="food.name">
          <div class="back" @click="hideFood">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <div class="title">{{food.name}}</div>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="new">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <transition name="fade">
            <div class="bug-wrapper" @click.stop="firstFood" v-show="!food.count || food.count === 0">
              <div class="buy">加入购物车</div>
            </div>
          </transition>
          <div class="cart-control-wrapper">
            <cart-control @addCart="addCart(food)" @decCart="decCart(food)" :food="food"></cart-control>
          </div>
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品介绍</h1>
          <p class="description">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <div class="rating-select-wrapper">
            <rating-select :onlyContent="onlyContent" :selectType="selectType" @modSelectType="modSelectType"
                      :desc="desc"  :count="count"   @modOnlyContent="modOnlyContent"></rating-select>
          </div>
          <div class="rating-wrapper">
            <ul v-show="typeRatings && typeRatings.length">
              <li class="rating-item border-1px" v-for="rating in typeRatings">
                <div class="info-bar">
                  <span class="time">{{rating.rateTime}}</span>
                  <div class="user">
                    <span class="name">{{rating.username}}</span>
                    <img class="avatar" :src="rating.avatar" :alt="rating.username">
                  </div>
                </div>
                <p class="text">
                  <span :class="{'icon-thumb_up':rating.rateType ===0 ,'icon-thumb_down':rating.rateType === 1}"></span>
                  {{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!typeRatings || !typeRatings.length">
              暂时没有评价...
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl";
  .food
    position fixed
    left 0
    top 0
    width 100%
    bottom 0.96rem
    background-color #ffffff
    overflow hidden
    z-index 45
    transition all 0.2s linear
    transform translate3d(0,0,0)
    &.move-enter-active,&.move-leave-active
      opacity 1
    &.move-enter,&.move-leave-active
      transform translate3d(100%,0,0)
      opacity 0
    .img-header
      position relative
      width 100%
      height 0
      padding-top 100%
      img
        position absolute
        left 0
        top 0
        width 100%
        height 100%
      .back
        position absolute
        left 0.2rem
        top  0.2rem
        width 0.5rem
        height 0.5rem
        line-height 0.5rem
        text-align center
        border-radius 50%
        background rgba(7,17,27,0.3)
        .icon-arrow_lift
          display inline-block
          font-size 0.3rem
          color rgb(255,255,255)
          margin 0.12rem 0.04rem 0 0
    .content
      position relative
      padding 0.36rem
      .title
        margin-bottom 0.16rem
        line-height 0.28rem
        font-size 0.28rem
        font-weight 700
        color rgb(7,17,27)
      .detail
        margin-bottom 0.36rem
        font-size 0
        color rgb(147,153,159)
        .sell-count
          display inline-block
          line-height 0.2rem
          margin-right 0.24rem
          font-size 0.2rem
        .rating
          display inline-block
          line-height 0.2rem
          font-size 0.2rem
      .price
        font-size 0
        .new
          display inline-block
          line-height 0.48rem
          font-size 0.28rem
          font-weight 700
          color rgb(240,20,20)
          margin-right 0.24rem
        .old
          display inline-block
          line-height 0.48rem
          font-size 0.28rem
          font-weight 200
          text-decoration line-through
          color rgb(147,153,159)
      .bug-wrapper
        position absolute
        bottom 0.36rem
        right 0.36rem
        z-index 20
        padding 0.06rem
        transition all 0.1s linear
        opacity 1
        &.fade-enter-active,&.fade-leave-active
          opacity 1
        &.fade-enter,&.fade-enter-leave-active
          opacity 0
        .buy
          width 1.48rem
          height 0.48rem
          line-height 0.48rem
          border-radius 0.12rem
          font-size 0.2rem
          color rgb(255,255,255)
          text-align center
          background-color rgb(0,160,220)
      .cart-control-wrapper
        position absolute
        bottom 0.36rem
        right 0.36rem
        z-index 10
    .info
      margin 0.36rem
      .title
        font-size 0.28rem
        font-weight 200
        line-height 0.28rem
        color rgb(7,17,27)
      .description
        font-size 0.24rem
        font-weight 200
        line-height 0.48rem
        color rgb(77,85,93)
        padding 0.12rem 0.16rem 0.36rem
    .rating
      .title
        font-size 0.28rem
        font-weight 200
        line-height 0.28rem
        color rgb(7,17,27)
        margin 0.36rem 0.36rem 0
      .rating-select-wrapper
        margin 0.36rem 0.36rem 0
      .rating-wrapper
        padding  0 0.36rem
        border-top-1px(rgba(7,17,27,0.4))
        .rating-item
          padding 0.32rem 0
          border-bottom-1px(rgba(7,17,27,0.1))
          &:last-child
            border-none()
          .info-bar
            display flex
            justify-content space-between
            color rgb(147,153,159)
            font-size 0.2rem
            line-height 0.24rem
            .user
              display flex
              .name
                margin-right 0.12rem
              .avatar
                width 0.24rem
                height 0.24rem
          .text
            margin-top 0.12rem
            color rgb(7,17,27)
            font-size 0.24rem
            line-height 0.32rem
            span
              font-size 0.24rem
              line-height 0.48rem
              margin-right 0.08rem
              color rgb(147,153,159)
              &.icon-thumb_up
                color rgb(0,160,220)
        .no-rating
          width 100%
          text-align center
          height 1rem
          line-height 1rem
          font-size 0.32rem
          color rgb(7,17,27)
</style>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import cartControl from './../cartcontrol/cartcontrol.vue';
  import split from './../split/split.vue';
  import ratingSelect from './../ratingselect/ratingselect.vue';

  export default {
    props: {
      food: {
        type: Object
      }
    },
    computed: {
      typeRatings () { // 按类型获取评价数据列表
        if (this.food.ratings && this.food.ratings.length) {
          let ratings = [];
          if (this.onlyContent) { // 过滤获取有内容的评价
            this.food.ratings.forEach((item) => {
              if (item.text) {
                ratings.push(item);
              }
            });
          } else {
            ratings = this.food.ratings;
          }
          // 根据评价类型过滤
          if (this.selectType === 0) {
            return ratings;
          } else if (this.selectType === 1) {
            return ratings.filter((item) => {
              return item.rateType === 0;
            });
          } else if (this.selectType === 2) {
            return ratings.filter((item) => {
              return item.rateType === 1;
            });
          }
        }
        return [];
      },
      count () {
        let count = {
          all: 0,
          positive: 0,
          negative: 0
        };
        if (this.food.ratings && this.food.ratings.length) {
          let ratings = [];
          if (this.onlyContent) { // 过滤获取有内容的评价
            this.food.ratings.forEach((item) => {
              if (item.text) {
                ratings.push(item);
              }
            });
          } else {
            ratings = this.food.ratings;
          }
          // 根据评价类型过滤
          count.all = ratings.length;
          count.positive = ratings.filter((item) => {
            return item.rateType === 0;
          }).length;
          count.negative = ratings.filter((item) => {
            return item.rateType === 1;
          }).length;
        }
        return count;
      }
    },
    components: {
      cartControl,
      split,
      ratingSelect
    },
    data () {
      return {
        showFlag: false, // 默认不显示商品的详情组件
        selectType: 0, // 默认0：选择全部
        onlyContent: false, // 默认不勾选 --只看有内容的评价
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      _initScroll () { // 初始化滚动高度
        this.$nextTick(() => {
          if (this.scroll) {
            this.scroll.refresh();
          } else {
            this.scroll = new BScroll(this.$refs.food, {click: true});
          }
        });
      },
      showFood () { // 展示商品详情
        this.showFlag = true;
        this._initScroll();
      },
      hideFood () { // 隐藏商品详情
        this.showFlag = false;
      },
      firstFood () { // 加入购物车
        if (this.food.count) { // 购买数量自增
          this.food.count += 1;
        } else { // 首次购买
          Vue.set(this.food, 'count', 1);
        }
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
      modSelectType (type) {
        this.selectType = type;
        this._initScroll();
      },
      modOnlyContent (onlyContent) { //  // 修改只看有内容的评价的按钮
        this.onlyContent = onlyContent;
        this._initScroll();
      }
    }
  };
</script>
