<template>
  <div id="app">
    <!--头部-->
    <the-header :seller="seller"></the-header>
    <!--tab-->
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods" active-class="active">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings" active-class="active">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/stores" active-class="active">商家</router-link>
      </div>
    </div>
    <!--内容区域-->
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
    <div class="footer">
      <keep-alive>
        <cart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></cart>
      </keep-alive>
    </div>
  </div>
</template>

<script>
  import TheHeader from 'components/TheHeader'
  import Cart from 'components/Cart'

  const ERR_OK = 0

  export default {
    data () {
      return {
        goods: {},
        seller: {}
      }
    },
    created () {
      this.$http.get('/api/seller').then((res) => {
        if (res.body.errno === ERR_OK) {
          this.seller = res.body.data
        } else {
          console.log('App.vue:', '获取数据异常')
        }
      }, (err) => {
        console.log(err)
      })
    },
    components: {
      'the-header': TheHeader,
      'cart': Cart
    },
    name: 'App'
  }
</script>

<style scoped lang="scss">
  @import "common/styles/base";

  .tab {
    display: flex;
    display: -webkit-flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
    align-items: center;
    height: 40px;
    .tab-item {
      flex: 1;
      align-self: center;
      height: inherit;
      display: flex;
      justify-content: center;
      align-items: center;
      @include border-1px(rgba(7, 17, 27, 0.1));
      & > a {
        font-size: 14px;
        line-height: 14px;
        text-underline: none;
        color: rgb(77, 85, 93);
        display: block;
        justify-content: center;
        align-content: center;
        &.active {
          color: rgb(240, 20, 20)
        }
      }
    }
  }

  .footer {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    height: 62px;
  }
</style>
