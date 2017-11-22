<template>
  <div class="rating-select">
    <div class="rating-type border-1px">
      <span class="all" @click.stop="select(0,$event)" :class="{'active':selectType === 0}">{{desc.all}}<span class="count">{{count.all}}</span></span>
      <span class="positive" @click.stop="select(1,$event)" :class="{'active':selectType === 1}">{{desc.positive}}<span class="count">{{count.positive}}</span></span>
      <span class="negative" @click.stop="select(2,$event)" :class="{'active':selectType === 2}">{{desc.negative}}<span class="count">{{count.negative}}</span></span>
    </div>
    <div class="switch" :class="{'on':onlyContent}">
      <span class="icon-check_circle" @click.stop="modOnlyContent"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl";
  .rating-select
    .rating-type
      padding 0.24rem 0 0.36rem
      font-size 0
      border-bottom-1px(rgba(7,17,27,0.1))
      .all,.positive,.negative
        display inline-block
        font-size 0.24rem
        line-height 0.32rem
        border-radius 0.04rem
        margin-right 0.16rem
        text-align center
        padding 0.16rem 0.24rem
        color rgb(77,85,93)
        background-color rgba(77,85,93,0.2)
        &.active
          color rgb(255,255,255)
          background-color rgb(0,160,220)
      .count
        font-size 0.16rem
        margin-left 0.04rem
    .switch
      margin 0.24rem 0
      font-size 0
      line-height 0.48rem
      color rgb(147,153,159)
      &.on
        .icon-check_circle
          color #00c850
      .icon-check_circle,.text
        display inline-block
        vertical-align top
      .icon-check_circle
        font-size 0.48rem
      .text
        font-size 0.24rem
        margin-left 0.08rem
</style>
<script type="text/ecmascript-6">
  const ALL = 0;
//  const POSITIVE = 1;
//  const NEGATIVE = 2;
  export default {
    data () {
      return {
      };
    },
    props: {
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default () {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          };
        }
      },
      count: {
        type: Object,
        default () {
          return {
            all: 0,
            positive: 0,
            negative: 0
          };
        }
      }
    },
    methods: {
      select (type, event) { // 选择按钮并激活
        if (event._constructed) {
          if (this.selectType === type) { // 识别是否点击激活按钮
            return;
          }
          // 通知父组件修改selectType
          this.$emit('modSelectType', type);
        }
      },
      modOnlyContent () { // 修改只看有内容的评价的按钮
        this.$emit('modOnlyContent', !this.onlyContent);
      }
    }
  };
</script>
