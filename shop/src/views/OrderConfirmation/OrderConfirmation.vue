<template>
<div class="wrapper">
    <TopArea />
    <ProductList :productList = productList :shopName= shopName />
    <Order />
</div>
</template>

<script>
import TopArea from './TopArea.vue'
import ProductList from '../../components/productList.vue'
import Order from './Order.vue'
import { useRoute } from 'vue-router'
import { useCommonCartEffect } from '../../effects/cartEffects.js'
export default {
  name: 'OrderConfirmation',
  components: { TopArea, ProductList, Order },
  setup () {
    const route = useRoute()
    const shopId = route.params.id
    const { shopName, productList } = useCommonCartEffect(shopId)
    return { shopName, productList }
  }
}
</script>

<style lang="scss" scoped>
.wrapper{
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    background-color: #eee;
    overflow-y: scroll;
}
.product{
    &__wrapper{
        position: absolute;
        left: 0;
        right: 0;
        bottom: .6rem;
        top: 2.6rem;
    }
}
</style>
