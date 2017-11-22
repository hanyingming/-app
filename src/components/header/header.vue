<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" :alt="seller.name">
      </div>
      <div class="content">
        <div class="title">
          <div class="brand"></div>
          <div class="name">{{seller.name}}</div>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="supports" v-if="seller.supports">
          <div class="icon" :class="classMap[seller.supports[0].type]"></div>
          <div class="text">{{seller.supports[0].description}}</div>
        </div>
        <div class="supports-count" v-if="seller.supports" @click="showDetail=true">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" :alt="seller.name">
    </div>
    <transition name="fade">
      <div class="detail" v-show="showDetail">
        <div class="detail-wrapper">
          <div class="detail-main">
            <div class="name">{{seller.name}}</div>
            <div class="star-wrapper">
              <star :size="48" :score="4"></star>
            </div>
            <div class="title">
              <span class="line"></span>
              <span class="text">优惠信息</span>
              <span class="line"></span>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="description">{{item.description}}</span>
              </li>
            </ul>
            <div class="title">
              <span class="line"></span>
              <span class="text">商家公告</span>
              <span class="line"></span>
            </div>
            <p class="bulletin">
              {{seller.bulletin}}
            </p>
          </div>
        </div>
        <div class="detail-close" @click="showDetail=false">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  @import './../../common/stylus/mixin';

  .header
    position relative
    overflow hidden
    .content-wrapper
      display flex
      background-color rgba(7,17,25,0.5)
      color rgb(255,255,255)
      .avatar
        margin 0.48rem 0.32rem 0.36rem 0.48rem
        width 1.28rem
        height 1.28rem
        border-radius 0.04rem;
        overflow hidden
      .avatar > img
        width 100%
        height 100%
      .content
        position relative
        flex-grow 1
        margin 0.52rem 0 0 0
        .title
          display flex
          .brand
            width 0.6rem
            height 0.36rem
            bg-img('brand')
            background-size cover
            background-repeat no-repeat
            margin 0.04rem 0.12rem 0.16rem 0
          .name
            font-size 0.32rem
            font-weight bold
            height 0.36rem
            line-height 0.36rem
        .description
          height 0.24rem
          line-height 0.24rem
          font-weight 200
          font-size 0.24rem
        .supports
          display flex
          margin 0.2rem 0 0 0
          .icon
            width 0.24rem
            height 0.24rem
            margin-right 0.08rem
            background-size cover
            background-repeat no-repeat
            &.decrease
              bg-img('decrease_1')
            &.discount
              bg-img('discount_1')
            &.guarantee
              bg-img('guarantee_1')
            &.invoice
              bg-img('invoice_1')
            &.special
              bg-img('special_1')
          .text
            font-size 0.2rem
            font-weight 200
        .supports-count
          position absolute
          box-sizing border-box
          right 0.24rem
          bottom 0.36rem
          height 0.48rem
          padding 0.12rem 0.16rem
          border-radius 0.32rem
          background-color rgba(0,0,0,0.2)
          font-size 0
          .count
            display inline-block
            vertical-align top
            font-size 0.2rem
            font-weight 200
            line-height 0.24rem
            margin-right 0.04rem
          .icon-keyboard_arrow_right
            display inline-block
            vertical-align top
            font-size 0.2rem
            line-height 0.24rem
            font-weight 200
    .bulletin-wrapper
      display flex
      height 0.56rem
      padding 0 0.24rem
      background-color rgba(7,17,27,0.2)
      align-items center
      .bulletin-title
        flex-shrink 0
        width 0.44rem
        height 0.22rem
        bg-img('bulletin')
        background-size cover
        background-repeat no-repeat
      .bulletin-text
        font-size 0.2rem
        color rgb(255,255,255)
        font-weight 200
        margin 0.04rem 0.08rem 0
        line-height 0.56rem
        overflow hidden
        white-space nowrap
        text-overflow ellipsis
      .icon-keyboard_arrow_right
        font-size 0.2rem
        color rgb(255,255,255)
        font-weight 200
    .background
      position absolute
      z-index -1
      left 0
      top 0
      width 100%
      height 100%
      filter blur(10px)
      & > img
        width 100%
        height 100%
    .detail
      position fixed
      left 0
      top 0
      z-index 100
      width 100%
      height 100%
      background rgba(7,17,27,0.8)
      overflow auto
      backdrop-filter blur(10px)
      transition all 0.2s ease-in
      &.fade-enter-active,&.fade-leave-active
        opacity 1
      &.fade-enter,&.fade-leave-active
        opacity 0
      .detail-wrapper
        min-height 100%
        display inline-block
        width 100%
        .detail-main
          margin 1.28rem 0.72rem 0
          padding-bottom 1.28rem
          color rgb(255,255,255)
          .name
            font-size 0.32rem
            font-weight 700
            line-height 0.32rem
            text-align center
          .star-wrapper
            height 0.48rem
            text-align center
            margin 0.32rem 0 0.56rem
          .title
            display flex
            font-weight 700
            font-size 0.24rem
            .line
              flex 1
              position relative
              top -0.12rem
              border-bottom  1px solid #cccccc
            .text
              padding 0 0.24rem
          .supports
            margin 0.48rem 0.24rem 0.56rem
            font-size 0
            .support-item
              height 0.32rem
              line-height 0.32rem
              margin-top 0.24rem
              &:first-child
                margin-top 0
              .icon
                display inline-block
                width 0.32rem
                height 0.32rem
                vertical-align top
                margin-right 0.12rem
                background-size cover
                background-repeat no-repeat
                &.decrease
                  bg-img('decrease_1')
                &.discount
                  bg-img('discount_1')
                &.guarantee
                  bg-img('guarantee_1')
                &.invoice
                  bg-img('invoice_1')
                &.special
                  bg-img('special_1')
              .description
                font-weight 200
                font-size 0.24rem
          .bulletin
            margin-top 0.48rem
            padding 0 0.24rem
            font-size 0.24rem
            line-height 0.48rem
            font-weight 200
            color rgb(255,255,255)
      .detail-close
        margin -1.28rem auto 0
        width 0.64rem
        height 0.64rem
        font-size 0.64rem
        color rgba(255,255,255,0.5)
</style>
<script type="text/ecmascript-6">
  import star from '@/components/star/star';
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        classMap: ['decrease', 'discount', 'guarantee', 'invoice', 'special'],
        showDetail: false
      };
    },
    components: {star}
  };
</script>
