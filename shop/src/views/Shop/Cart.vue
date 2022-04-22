<template>
  <div class="mask" @click="() => changeProductDisplay(total)" v-show=" isShow && total > 0 "></div>
    <div class="cart">
        <div class="category">
            <div class="product" v-show="isShow && total > 0">
              <div class="product__header">
                <div class="product__header__all" @click="() => setCartItemsChecked(shopId)">
                  <span class="product__header__icon" v-html="allChecked ? '√' : '×'"></span>
                  全选
                </div>
                <div class="product__header__clear"><span @click="() => cleanCartProducts(shopId)">清空购物车</span></div>
              </div>
                <div class="product__item" v-for="item in productList" :key="item._id">
                    <div class="product__item__checked"
                    v-html="item.check ? '√' : '×'"
                    @click="() => changeCartItemChecked(shopId, item._id)"
                    >
                    </div>
                    <img class="product__item__img" :src="item.imgUrl" />
                    <div class="product__item__detail">
                        <h4 class="product__item__title">{{item.name}}</h4>
                        <p class="product__item__price">
                            <span class="product__item__price__yen">&yen;{{item.price}}</span>
                            <span class="product__item__price__origin">&yen;{{item.oldPrice}}</span>
                        </p>
                    </div>
                    <div class="product__item__number">
                        <span class="product__item__number__minus" @click="() => {changeCartItemInfo(shopId, item._id, item, -1)}">-</span>
                        {{item.count || 0}}
                        <span class="product__item__number__plus" @click="() => {changeCartItemInfo(shopId, item._id, item, 1)}">+</span>
                    </div>
                </div>
        </div>
        </div>
        <div class="check">
              <div class="check__icon" @click="() => changeProductDisplay(total)">
                  <img src="http://www.dell-lee.com/imgs/vue3/basket.png" class="check__icon__img" />
                  <div class="check__icon__tag">{{total}}</div>
              </div>
            <div class="check__info">
                总计：<span class="check__info__price"> &yen;{{price}}</span>
            </div>
            <div class="check__btn" v-show="total > 0">
              <router-link :to="{ path:`/OrderConfirmation/${shopId}`}">去结算</router-link>
            </div>
        </div>
    </div>
</template>

<script>
import { computed, ref } from 'vue'
import { useRoute } from 'vue-router'
import { useStore } from 'vuex'
import { useCommonCartEffect } from '../../effects/cartEffects.js'
const useTotalEffect = (shopId, cartList) => {
  const total = computed(() => {
    const productList = cartList[shopId]?.productList
    let count = 0
    if (productList) {
      for (const i in productList) {
        const product = productList[i]
        if (product.check) { count += product.count }
      }
    }
    return count
  })
  return { total }
}

const useChangeCartItemEffect = (store, shopId, cartList) => {
  const changeCartItemChecked = (shopId, productId) => {
    store.commit('changeCartItemChecked', { shopId, productId })
  }

  const cleanCartProducts = (shopId) => {
    store.commit('cleanCartProducts', { shopId })
  }

  const allChecked = computed(() => {
    const productList = cartList[shopId]?.productList
    let result = true
    if (productList) {
      for (const i in productList) {
        const product = productList[i]
        if (product.count > 0 && !product.check) { result = false }
      }
    }
    return result
  })
  const setCartItemsChecked = (shopId) => {
    store.commit('setCartItemsChecked', { shopId })
  }

  return { changeCartItemChecked, cleanCartProducts, allChecked, setCartItemsChecked }
}
const useChangeProductDisplay = () => {
  const isShow = ref(false)
  const changeProductDisplay = (total) => {
    if (total > 0) {
      isShow.value = !isShow.value
    } else {
      isShow.value = false
    }
  }
  return { changeProductDisplay, isShow }
}

export default {
  name: 'Cart',
  setup () {
    const store = useStore()
    const route = useRoute()
    const shopId = route.params.id
    const cartList = store.state.cartList
    const { total } = useTotalEffect(shopId, cartList)
    const { changeCartItemChecked, cleanCartProducts, allChecked, setCartItemsChecked } = useChangeCartItemEffect(store, shopId, cartList)
    const { changeCartItemInfo, price, productList } = useCommonCartEffect(shopId)
    const { changeProductDisplay, isShow } = useChangeProductDisplay()
    return { total, price, productList, changeCartItemInfo, shopId, changeCartItemChecked, changeProductDisplay, isShow, cleanCartProducts, allChecked, setCartItemsChecked }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';
.mask{
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    background: rgba( 0, 0, 0, .5);
    z-index: 1;
}
.cart{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 2;
    background-color: $bgColor;
}
.product{
    overflow-y: scroll;
    flex: 1;
    background-color: $bgColor;
    &__header{
      display: flex;
      line-height: .52rem;
      border-bottom: 1px solid $content-bgColor;
      font-size: .14rem;
      color: $content-fontcolor;
      &__all{
        width: .64rem;
        margin-left: .18rem;
      }
      &__icon{
        color: $btn-bgColor;
        font-size: .2rem;
      }
      &__clear{
        flex: 1;
        margin-right: .16rem;
        text-align: right;
      }
    }
    &__item{
        position: relative;
        display: flex;
        padding: .12rem 0;
        margin: 0 .16rem;
        border-bottom: 0.01rem solid $content-bgColor;
        &__checked{
            line-height: .5rem;
            margin-right: .2rem;
            color: $btn-bgColor;
            font-size: .2rem;
        }
        &__detail{
            overflow: hidden;
        }
        &__img{
            width: .46rem;
            height: .46rem;
            margin-right: .16rem;
        }
        &__title{
            margin: 0;
            line-height: .2rem;
            font-size: .14rem;
            color: $content-fontcolor;
            @include ellipsis;
        }
        &__price{
            margin: 0.06rem 0 0 0;
            line-height: .2rem;
            font-size: .14rem;
            color: $hightlight-fontColor;
            &__yen{
                font-size: .12rem;
            }
            &__origin{
                margin-left: .06rem;
                line-height: .2rem;
                font-size: .12rem;
                color: $light-fontColor;
                text-decoration: line-through;
            }
        }
        &__number{
            position: absolute;
            right: 0;
            bottom: .12rem;
            &__minus, &__plus{
                display: inline-block;
                width: .2rem;
                height: .2rem;
                line-height: .16rem;
                border-radius: 50%;
                font-size: .2rem;
                text-align: center;
            }
            &__minus{
                border: .01rem solid $medium-fontColor;
                color: $medium-fontColor;
                margin-right: .05rem;
            }
            &__plus{
                background: $btn-bgColor;
                color: $bgColor;
                margin-left: .05rem;
            }
        }
    }
}
.check{
    display: flex;
    height: .49rem;
    border: .01rem solid $content-bgColor;
    line-height: .5rem;
    &__icon{
        position: relative;
        width: .84rem;
        &__img{
            display: block;
            margin: .12rem auto;
            width: .28rem;
            height: .26rem;
        }
        &__tag{
            position: absolute;
            left: .46rem;
            top: .04rem;
            width: .2rem;
            height: .2rem;
            line-height: .2rem;
            background-color: $hightlight-fontColor;
            border-radius: 50%;
            font-size: .12rem;
            text-align: center;
            color: $bgColor;
            transform: scale(.5);
            transform-origin: left center;
        }
    }
    &__info{
        flex: 1;
        color: $content-fontcolor;
        font-size: .12rem;
        &__price{
            color: $hightlight-fontColor;
            font-size: .18rem;
        }
    }
    &__btn{
        width: .98rem;
        background-color: #4fb0f9;
        text-align: center;
        font-size: .14rem;
        a{
          color: $bgColor;
          text-decoration: none;
        }
    }
}
</style>
