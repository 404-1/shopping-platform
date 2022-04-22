<template>
    <div class="wrapper">
        <div class="search">
            <div class="search__back iconfont" @click="handleBackClick">&#xe600;</div>
            <div class="search__content">
                <span class="search__content__icon iconfont">&#xe626;</span>
                <input class="search__content__input" placeholder="请输入商品名称"/>
            </div>
        </div>
        <ShopInfo :item="item" :hideBorder= 'true' />
    </div>
    <Content :shopName="item.name" />
    <Cart />
</template>

<script>
import { useRouter, useRoute } from 'vue-router'
import ShopInfo from '../../components/ShopInfo.vue'
import Content from './Content.vue'
import Cart from './Cart.vue'
import { get } from '../../utils/request.js'
import { reactive, toRefs } from 'vue'
const useShopInfoEffect = () => {
  const route = useRoute()
  const data = reactive({
    item: { _id: 1, name: '沃尔玛', imgUrl: 'http://www.dell-lee.com/imgs/vue3/near.png', sales: '1万+', expressLimit: '￥0', expressPrice: '￥5', slogan: 'Vip分享' }
  })
  const getShopInfo = async () => {
    const result = await get(`/api/shop/${route.params.id}`)
    if (result?.errno === 0 && result?.data?.length) {
      data.item = result.data
    }
  }
  const { item } = toRefs(data)
  return { item, getShopInfo }
}
const useBackClickEffect = () => {
  const router = useRouter()
  const handleBackClick = () => {
    router.back()
  }
  return { handleBackClick }
}
export default {
  name: 'Shop',
  components: { ShopInfo, Content, Cart },
  setup () {
    const { item, getShopInfo } = useShopInfoEffect()
    const { handleBackClick } = useBackClickEffect()
    getShopInfo()
    return { item, handleBackClick }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.wrapper{
    padding: 0 .18rem;
}
.search{
    display: flex;
    margin: .14rem 0 .04rem 0;
    height: .32rem;
    &__back{
        width: .3rem;
        font-size: .24rem;
        color: #B6B6B6;
        line-height: .29rem;
    }
    &__content{
        display: flex;
        flex: 1;
        line-height: .32rem;
        background: $search-bgColor;
        border-radius: .16rem;
        &__icon{
            color:$search-fontcolor;
            padding: 0 .08rem 0 .16rem;
        }
        &__input{
            width: 100%;
            padding-right: .2rem;
            border: none;
            outline: none;
            background: none;
            height: .32rem;
            font-size: .14rem;
            color: $content-fontcolor;
            &::placeholder{
                color: $content-fontcolor;
            }
        }
    }
}
</style>
