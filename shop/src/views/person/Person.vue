<template>
  <div class="wrapper">
    <div class="personalData">
      <div class="personalData__top"></div>
      <div class="personalData__bevellLeft"></div>
      <div class="personalData__bevellright"></div>
      <div class="personalData__content">
        <img class="personalData__content__photo" src="../../image/avatar.jpg"/>
        <div class="personalData__content__user">
          <div class="personalData__content__user__userName">热心市民李四</div>
          <div class="personalData__content__user__userId">ID：123456789</div>
        </div>
        <div class="personalData__wallet">
          <div class="personalData__wallet__list" v-for="(item, index) in walletList" :key="index">
            <div class="personalData__wallet__list__text">{{item.text}}</div>
            <div class="personalData__wallet__list__num">{{item.num}}</div>
          </div>
        </div>
      </div>
    </div>
    <div class="other">
      <div class="other__list">
        <div class="other__list__content"  v-for="(item, index) in list" :key="index">
          <span class="other__list__content__iconfont iconfont" v-html="item.icon"></span>
          <span class="other__list__content__text">{{item.text}}</span>
          <span class="other__list__content__edit iconfont">&#xe8f1;</span>
        </div>
      </div>
    </div>
  </div>
  <Docker :currentIndex="3"/>
</template>

<script>
import Docker from '../../components/Docker.vue'

export default {
  name: 'Person',
  components: { Docker },
  setup () {
    const walletList = [
      { text: '红包', num: '218' },
      { text: '优惠券', num: '12张' },
      { text: '鲜豆', num: '88' },
      { text: '白条', num: '1000' }
    ]
    const list = [
      { icon: '&#xe607;', text: '我的钱包', to: { name: 'Home' } },
      { icon: '&#xe616;', text: '我的地址', to: { name: 'CartList' } },
      { icon: '&#xe609;', text: '客服与帮助', to: { name: 'OrderList' } }
    ]
    return { walletList, list }
  }
}
</script>

<style lang="scss" scoped>
@import '../../style/viriables.scss';
.wrapper {
  overflow-y: auto;
  position: absolute;
  left: 0;
  top: 0;
  bottom: .6rem;
  right: 0;
  background: #FFFFFF;
}
.personalData {
  position: relative;
  height: 3.11rem;
  font-size: .16rem;
  color: $content-fontcolor;
  text-align: center;
  &__top{
    position: relative;
    height: 2.2rem;
    background: linear-gradient(239deg, #3A6FF3 0%, #50C7FB 100%);
  }
  &__bevellLeft{
    position: absolute;
    bottom: 0;
    right: 0;
    width: 0;
    height: 0;
    border-bottom: 1.3rem solid #FFFFFF;
    border-left: 2rem solid transparent;
  }
  &__bevellright{
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 0;
    border-bottom: 1.3rem solid #FFFFFF;
    border-right: 2rem solid transparent;
    // width: 100%;
    // background: linear-gradient(145deg, transparent 110px, #fff 0) right, linear-gradient(-145deg, transparent 110px, #fff 0) left;
    // background-size: 50% 100%;
    // background-repeat: no-repeat;
  }
  &__content{
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 2.03rem;
    margin: 0 .18rem;
    background: #FFFFFF;
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.08);
    box-sizing: border-box;
    border-radius: 8px;
    text-align:center;
     &__photo{
      position: absolute;
      right: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: #456BD9;
      border-radius: 50%;
      height: .94rem;
      width: .94rem;
    }
    &__user{
      height: 1.26rem;
      box-sizing: border-box;
      font-family: PingFangSC-Regular;
      &__userName{
        padding: .59rem 0 .02rem 0;
        font-size: .24rem;
        color: #262628;
        letter-spacing: 0.19px;
        line-height: 36px;
      }
      &__userId{
        font-size: .14rem;
        color: #C1C0C9;
      }
    }
  }
  &__wallet{
    display: flex;
    height: .76rem;
    border-top: .01rem solid $content-bgColor;
    &__list{
      flex: 1;
      font-family: PingFangSC-Regular;
      &__text{
        margin: .13rem 0 .03rem 0;
        font-size: .12rem;
        color: #C1C0C9;
      }
      &__num{
        font-size: .2rem;
        color: #262628;
      }
    }
  }
}
.other{
  margin: .16rem .18rem 0 .18rem;
  background: #FFFFFF;
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.08);
  border-radius: 8px;
  &__list{
    display: flex;
    flex-direction:column;
    width: 100%;
    height: 1.46rem;
    line-height: .48rem;
    &__content{
      flex: 1;
      position: relative;
      &__iconfont{
        padding: 0 .21rem 0 .12rem;
      }
      &__text{
        font-family: PingFangSC-Regular;
        font-size: 14px;
        color: #262626;
      }
      &__edit{
        position: absolute;
        right: .16rem;
      }
    }
  }
}
</style>
