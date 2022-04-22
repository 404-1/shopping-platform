<template>
  <div class="nearby">
      <h3 class="nearby__title">附近店铺</h3>
      <router-link :to="`/Shop/${item._id}`"  v-for="item in nearbyList" :key= item._id>
        <ShopInfo :item="item" />
      </router-link>
    </div>
</template>

<script>
import ShopInfo from '../../components/ShopInfo.vue'
import { get } from '../../utils/request.js'
import { ref } from 'vue'
const useNearbyListEffect = () => {
  const nearbyList = ref([
    { _id: 1, name: '沃尔玛', imgUrl: 'http://www.dell-lee.com/imgs/vue3/near.png', sales: '1万+', expressLimit: '￥0', expressPrice: '￥5', slogan: 'Vip分享' }
  ])
  const getNearbyList = async () => {
    const result = await get('/api/shop/hot-list')
    if (result?.errno === 0 && result?.data?.length) {
      nearbyList.value = result.data
    }
  }
  return { nearbyList, getNearbyList }
}
export default {
  name: 'Nearby',
  components: { ShopInfo },
  setup () {
    const { nearbyList, getNearbyList } = useNearbyListEffect()
    getNearbyList()
    return { nearbyList }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.nearby{
    &__title{
      margin: .16rem 0 .02rem 0;
      font-size: .18rem;
      font-weight: normal;
      color: $content-fontcolor;
    }
    a{
      text-decoration: none;
    }
  }
</style>
