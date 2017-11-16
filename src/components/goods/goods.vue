<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul v-if="goods">
        <li v-for="(item, index) in goods" class="menu-item border-1px" :class="{current: currentIndex === index}" @click="selectMenu(index, $event)">
          <span class="text">
            <icon v-if="item.type > 0" :size="3" :type="item.type"></icon>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li class="food-list" v-for="item in goods" ref="foodList">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item" v-for="food in item.foods" @click="selectFood(food, $event)">
              <div class="icon"><img :src="food.icon" alt="icon" width="57" height="57"></div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extrl">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import icon from '../icon/icon.vue'
import shopcart from '../shopcart/shopcart.vue'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import food from '../food/food.vue'

const ERR_OK = 0

export default {
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    icon,
    shopcart,
    cartcontrol,
    food
  },
  computed: {
    currentIndex (index) {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    },
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  },
  created () {
    this.$http.get('/api/goods').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.goods = response.data
        this.$nextTick(() => {
          this._initScroll()
          this._calculateHeight()
        })
      }
    })
  },
  methods: {
    selectFood (food, event) {
      if (!event._constructed) {
        return
      }
      this.$refs.food.show()
      this.selectedFood = food
    },
    selectMenu (index, event) {
      if (!event._constructed) {
        return
      }
      let foodList = this.$refs.foodList
      let el = foodList[index]
      this.foodsScroll.scrollToElement(el, 300)
    },
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        click: true,
        probeType: 3
      })
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight () {
      let foodList = document.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i]
        height += item.clientHeight
        this.listHeight.push(height)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../common/scss/base.scss';
@import '../../common/scss/mixin.scss';

  .goods {
    position: absolute;
    top: 176px;
    bottom: 46px;
    display: flex;
    width: 100%;
    overflow: hidden;
    .menu-wrapper {
      flex: 0 0 80px;
      background-color: #f3f5f7;
      .menu-item {
        display: table;
        height: 54px;
        width: 56px;
        padding: 0 12px;
        line-height: 15px;
        &.current {
          position: relative;
          z-index: 10;
          margin-top: -1px;
          font-weight: 700;
          background-color: #fff;
          .text {
            @include border-none;
          }
        }
        .text {
          display: table-cell;
          width: 56px;
          vertical-align: middle;
          @include border-1px(rgba(7, 17, 27, .1));
          font-size: 12px;
        }
      }
    } 
    .foods-wrapper {
      flex: 1;
      .title {
        padding-left: 14px;
        height: 26px;
        border-left: 2px solid #d9dde1;
        line-height: 26px;
        color: rgb(147, 152, 159);
        font-size:12px;
        background-color: #f3f5f7;
      }
      .food-item {
        display: flex;
        padding-bottom: 28px;
        margin: 18px;
        @include border-1px(rgba(7, 17, 27, .2));
        &:last-child {
          @include border-none;
        }
        .icon {
          flex: 0 0 57;
          margin-right: 10px;
        }
        .content {
          position: relative;
          flex: 1;
          .name {
            margin: 2px 0 8px 0;
            line-height: 14px;
            font-size: 14px;
            color: rgb(7, 17, 27);
          }
          .desc,
          .extrl {
            line-height: 12px;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
          .desc {
            margin-bottom: 8px;
          }
          .extrl {
            .count {
              margin-right: 10px;
            }
          }
          .price {
            margin-top: 3px;
            line-height: 24px;
            font-weight: 700;
            .now {
              margin-right: 8px;
              font-size: 14px;
              color: rgb(240, 20, 20);
            }
            .old {
              font-size: 10px;
              text-decoration: line-through;
              color: rgb(147, 153, 159);
            }
          }
          .cartcontrol-wrapper {
            position: absolute;
            bottom: -6px;
            right: 0;
          }
        }
      }
    }
  }
</style>