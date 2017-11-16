<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper" :class="{highlight: totalCount > 0}">
          <div class="logo" :class="{highlight: totalCount > 0}"><i class="icon-shopping_cart"></i></div>
          <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{highlight: totalPrice > 0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" :class="payClass" @click.stop="pay">
        <div class="pay">{{payDesc}}</div>
      </div>
    </div>
    <transition name="moves" enter-active-class="animated slideInUp" leave-active-class="animated slideOutDown">
      <div class="shopcart-list" v-show="listShow">
        <div class="list-header">
          <div class="title">购物车</div>
          <div class="empty" @click="empty">清空</div>
        </div>
        <div class="list-content" ref="shopcartList">
          <ul>
            <li class="food" v-for="food in selectFoods">
              <span class="name">{{food.name}}</span>
              <div class="price"><span>￥{{food.count * food.price}}</span></div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
    <transition name="fade">
      <div class="list-mark" v-show="listShow" @click="hideList"></div>
    </transition>
  </div>
</template>

<script>
  import cartcontrol from '../cartcontrol/cartcontrol.vue'
  import BScroll from 'better-scroll'
  export default {
    data () {
      return {
        fold: true
      }
    },
    props: {
      selectFoods: {
        type: Array
      },
      minPrice: {
        type: Number
      },
      deliveryPrice: {
        type: Number
      }
    },
    components: {
      cartcontrol
    },
    computed: {
      totalCount () {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.count
        })
        return total
      },
      totalPrice () {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total
      },
      payDesc () {
        if (this.minPrice === undefined) {
          return ''
        }
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}起送`
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice
          return `还差￥${diff}起送`
        } else {
          return `结算`
        }
      },
      payClass () {
        if (this.totalPrive === undefined && this.minPrice === undefined) {
          return false
        }
        if (this.totalPrice < this.minPrice) {
          return 'not-enough'
        } else {
          return 'enough'
        }
      },
      listShow () {
        if (!this.totalCount) {
          this.fold = true
          return false
        }
        let show = !this.fold
        if (show) {
          this.$nextTick(() => {
            if (!this.shopcartScroll) {
              this.shopcartScroll = new BScroll(this.$refs.shopcartList, {
                click: true
              })
            } else {
              this.shopcartScroll.refresh()
            }
          })
        }
        return show
      }
    },
    methods: {
      toggleList () {
        if (!this.totalCount) {
          return
        }
        this.fold = !this.fold
      },
      empty () {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      hideList () {
        this.fold = true
      },
      pay () {
        if (this.totalPrice < this.minPrice) {
          return false
        }
        window.alert(`总计${this.totalPrice + this.deliveryPrice}`)
      }
    }
  }
</script>

<style lang="scss" scoped>
@import '../../common/scss/mixin.scss';
@import '../../common/scss/animate.scss';
  .fade-enter,
  .fade-leave-active {
    opacity: 0;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 1s;
  }
  .shopcart {
    position: fixed;
    left: 0;
    bottom: 0;
    height: 46px;
    width: 100%;
    z-index: 50;
    .content {
      display: flex;
      height: 48px;
      background-color: #141d27;
      .content-left {
        flex: 1;
        font-size: 0;
        .logo-wrapper {
          display: inline-block;
          position: relative;
          top: -8px;
          margin: 0 12px;
          width: 56px;
          height: 56px;
          border-radius: 50%;
          background-color: #2b343c;
          &.highlight {
            background-color: #00a0dc;
          }
          .logo {
            width: 44px; 
            height: 44px;
            border: 6px solid #141d27;
            border-radius: 50%;
            text-align: center;
            color: #80858a;
            &.highlight {
              color: #fff;
            }
            .icon-shopping_cart {
              line-height: 44px;
              font-size: 24px;
            }
          }
          .num {
            position: absolute;
            top: 0;
            left: 33px;
            width: 24px;
            height: 16px;
            line-height: 18px;
            border-radius: 16px;
            text-align: center;
            font-size: 9px;
            color: #fff;
            background-color: rgb(240, 20, 20);
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4)
          }
        }
        .price,
        .desc {
          display: inline-block;
          vertical-align: top;
          margin-top: 12px;
          line-height: 24px;
          color: rgba(255, 255, 255, .4);
          font-weight: 700;
        }
        .price {
          padding-right: 12px;
          border-right: 1px solid rgba(255, 255, 255, .1);
          font-size: 16px;
          &.highlight {
            color: #fff;
          }
        }
        .desc {
          padding-left: 12px;
          font-size: 10px;
        }

      }
      .content-right {
        flex: 0 0 105px;
        width: 105px;
        &.not-enough {
          color: rgba(255, 255, 255, .4);
          background-color: #2b333b;
        }
        &.enough {
          color: #fff;
          background-color: #00b43c;
        }
        .pay {
          height: 48px;
          line-height: 48px;
          text-align: center;
          font-size: 12px;
          font-weight: 700;
        }
      }
    }
    .shopcart-list {
      position: absolute;
      bottom: 48px;
      left: 0;
      width: 100%;
      max-height: 270px;
      z-index: -1;
      background-color: #fff;
      // transform: translate3d(0, -100%, 0);
      .list-header {
        padding: 0 18px;
        height: 40px;
        @include border-1px(rgba(7, 17, 27, .1));
        line-height: 40px;
        background-color: #f3f5f7;
        .title {
          float: left;
          line-height: 40px;
          font-size: 14px;
          font-weight: 200;
          color: rgb(7, 17, 27);
        }
        .empty {
          float: right;
          line-height: 40px;
          color: rgb(0, 160, 220);
        }
      }
      .list-content {
        max-height: 217px;
        overflow: hidden;
        li {
          position: relative;
          padding: 0 18px;
          height: 48px;
          @include border-1px(rgba(7, 17, 27, .1));
          line-height: 48px;
        }
        .name,
        .price,
        .cartcontrol-wrapper {
          display: inline-block;
        }
        .name {
          font-size: 14px;
          line-height: 24px;
        }
        .price {
          position: absolute;
          right: 110px;
          bottom: 0px;
          font-size: 10px;
          color: rgb(240, 20, 20);
        }
        .cartcontrol-wrapper {
          position: absolute;
          right: 18px;
          top: 5px;
        }
      }
    }
  }
  .list-mark {
    position: fixed;
    top: 0;
    left: 0;
    z-index: -3;
    height: 100%;
    width: 100%;
    background-color: rgba(7, 17, 27, .6);
  }
</style>