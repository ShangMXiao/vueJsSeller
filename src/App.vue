<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      
      <div class="tab-item border-1px">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import Header from './components/header/header.vue'
const ERR_OK = 0

export default {
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then((response) => {
      let result = response.body
      if (result.erron === ERR_OK) {
        this.seller = result.data
      }
    })
  },
  components: {
    'v-header': Header
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
    @import "common/sass/mixin";

    .tab{
      display: flex;
      width: 100%;
      height: 40px;
      line-height: 40px;
      @include border-1px(rgba(7, 17, 27, 0.1));
      .tab-item{
        flex: 1;
        text-align: center;
        & > a {
          display: block;
          font-size: 14px;
          color: rgb(77, 85, 93);
          &.active {
            color: rgb(240, 20, 20);
          }
        }
      }
    }
</style>
