<template>
  <div class="wrapper">
    <div class="title">我的购物车</div>
        <div v-for="(items, index) in cartList" :key="index">
          <ProductList :productList = items.productList :shopName= items.shopName />
        </div>
  </div>
    <Docker :currentIndex="1"/>
</template>

<script>
import Docker from '../../components/Docker.vue'
import ProductList from '../../components/productList.vue'
import { useStore } from 'vuex'
import { computed } from 'vue'
export default {
  name: 'CartList',
  components: { Docker, ProductList },
  setup () {
    const cartList = computed(() => {
      const store = useStore()
      const cartList = store.state.cartList
      const allShop = {}
      for (const i in cartList) {
        const productList = cartList[i].productList
        for (const z in productList) {
          const product = productList[z]
          if (product.count > 0) {
            allShop[i] = cartList[i]
          }
        }
      }
      return allShop
    })
    console.log(cartList.value)
    return { cartList }
  }
}
</script>
<style lang="scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';
.wrapper {
  overflow-y: auto;
  position: absolute;
  left: 0;
  top: 0;
  bottom: .6rem;
  right: 0;
  background: rgb(248, 248,248);
}
.title {
  line-height: .44rem;
  background: $bgColor;
  font-size: .16rem;
  color: $content-fontcolor;
  text-align: center;
}
</style>
