<template>
  <div id="app">
    <nav-header :seller="seller"></nav-header>
    <div class="tab border-1px">
      <div class="tab-item"><router-link to="/goods">商品</router-link></div>
      <div class="tab-item"><router-link to="/ratings">评论</router-link></div>
      <div class="tab-item"><router-link to="/seller">商家</router-link></div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
  import header from '@/components/header/header';
  export default {
    data () {
      return {
        seller: {}
      };
    },
    components: {
      NavHeader: header
    },
    mounted: function () {
      this.getSellerData();
    },
    methods: {
      getSellerData () {
        this.$http.get('/api/seller').then((res) => {
          this.seller = res.data.result;
        }).catch((err) => {
          console.log(err);
        });
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl";
  #app
    .tab
      display flex
      height 0.8rem
      line-height 0.8rem
      border-bottom-1px(rgba(7,17,21,0.1))
      .tab-item
        flex 1
        text-align center
        & > a
          display block
          font-size 0.28rem
          color rgb(77,85,93)
          &.router-link-active
            color rgb(240,20,20)
</style>
