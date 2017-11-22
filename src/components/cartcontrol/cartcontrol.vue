<template>
  <div class="cart-control" @click.stop>
    <transition name="move">
      <div class="cart-decrease icon-remove_circle_outline" @click="decCart" v-show="food.count>0"></div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  .cart-control
    font-size 0
    .cart-decrease,.cart-count,.cart-add
      display inline-block
      vertical-align top
    .cart-decrease
      transition all 0.2s linear
      transform rotate(180deg)
      &.move-enter-active, &.move-leave-active
        opacity 1
      &.move-enter, &.move-leave-active
        transform translate3d(24px,0,0)
        opacity 0
    .cart-decrease,.cart-add
      line-height 0.48rem
      font-size 0.48rem
      padding 0.06rem
      color rgb(0,160,220)
    .cart-count
      width 0.48rem
      height 0.6rem
      line-height 0.6rem
      text-align center
      font-size 0.24rem
      color rgb(147,153,159)
</style>
<script type="text/ecmascript-6">
  export default {
    props: {
      food: {
        type: Object
      }
    },
    mounted: function () {
    },
    data () {
      return {
      };
    },
    methods: {
      addCart () { // 增加购买商品的数量
        this.$emit('addCart');
      },
      decCart () { // 减少购买商品的数量
        this.$emit('decCart');
      }
    }
  };
</script>
