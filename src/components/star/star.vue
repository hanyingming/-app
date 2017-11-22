<template>
  <div class="star" :class="starSize">
    <span class="star-item" v-for="itemClass in itemClasses" :class="itemClass"></span>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl";

.star
  font-size 0
  display inline-block
  .star-item
    display inline-block
    background-repeat no-repeat
    background-size cover
  &.star-48
    .star-item
      width 0.4rem
      height 0.4rem
      margin-right 0.44rem
      &:last-child
        margin-right 0
      &.on
        bg-img('star48_on')
      &.half
        bg-img('star48_half')
      &.off
        bg-img('star48_off')
  &.star-36
    .star-item
      width 0.3rem
      height 0.3rem
      margin-right 0.12rem
      &:last-child
        margin-right 0
      &.on
        bg-img('star36_on')
      &.half
        bg-img('star36_half')
      &.off
        bg-img('star36_off')
  &.star-24
    .star-item
      width 0.2rem
      height 0.2rem
      margin-right 0.06rem
      &:last-child
        margin-right 0
      &.on
        bg-img('star24_on')
      &.half
        bg-img('star24_half')
      &.off
        bg-img('star24_off')
</style>
<script type="text/ecmascript-6">
  const LENG = 5;
  const ON = 'on';
  const OFF = 'off';
  const HALF = 'half';

  export default {
    props: {
      size: {
        type: Number,
        default: 24
      },
      score: {
        type: Number,
        default: 0
      }
    },
    computed: {
      starSize () {
        return 'star-' + this.size;
      },
      itemClasses () {
        let result = [];
        let score = Math.floor(this.score * 2) / 2;
        let hasDecimal = score % 1 !== 0;
        let integer = Math.floor(score);
        for (let i = 0; i < integer; i++) {
          result.push(ON);
        }
        if (hasDecimal) {
          result.push(HALF);
        }
        while (result.length < LENG) {
          result.push(OFF);
        }
        return result;
      }
    }
  };
</script>
