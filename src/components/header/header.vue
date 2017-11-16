<template>
  <div class="header">
  	<div class="content-wrapper">
			<div class="avatar">
				<img :src="seller.avatar" alt="avatar" height="64p" width="64">
			</div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <div class="name">{{seller.name}}</div>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <!-- <span class="icon" :class="classMap[seller.supports[0].type]"></span> -->
          <icon :size="1" :type="seller.supports[0].type"></icon>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
        <div v-if="seller.supports" class="supports-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="avatar" height="100%" width="100%">
    </div>
    <transition name="fade">
      <div class="detail clearfix" v-show="detailShow">
        <div class="detail-wrapper">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <name :name="'优惠信息'"></name> 
            <ul class="supports">
              <li v-for="item in seller.supports" class="support-item">
                <!-- <span class="icon" :class="classMap[item.type]"></span> -->
                <icon :size="2" :type="item.type"></icon>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <name :name="'商家公布'"></name> 
            <div class="bulletin">
              <p class="content" id="inner-content-div">
                <!-- {{seller.bulletin}} -->
                粥品香坊其烹饪粥料的秘方源于中国千年古法，在融和现代制作工艺，由世界烹饪大师屈浩先生领衔研发。坚守纯天然、0添加的良心品质深得消费者青睐，发展至今成为粥类的引领品牌。是2008年奥运会和2013年园博会指定餐饮服务商。粥品香坊其烹饪粥料的秘方源于中国千年古法，在融和现代制作工艺，由世界烹饪大师屈浩先生领衔研发。坚守纯天然、0添加的良心品质深得消费者青睐，发展至今成为粥类的引领品牌。是2008年奥运会和2013年园博会指定餐饮服务商。粥品香坊其烹饪粥料的秘方源于中国千年古法，在融和现代制作工艺，由世界烹饪大师屈浩先生领衔研发。坚守纯天然、0添加的良心品质深得消费者青睐，发展至今成为粥类的引领品牌。是2008年奥运会和2013年园博会指定餐饮服务商。
              </p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import star from '../star/star.vue'
import name from '../title/title.vue'
import icon from '../icon/icon.vue'
// import $ from 'jquery'
// import slimScroll from 'jquery-slimscroll'

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
  components: {
    star,
    name,
    icon
  }
}
</script>

<style lang="scss" scoped>
@import '../../common/scss/base.scss';
@import '../../common/scss/mixin.scss';
// @import '../../common/scss/icon.scss';
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity .3s;
  }
  .fade-enter,
  .fade-leave-active {
    opacity: 0;
  }
  .header {
    position: relative;
    color: #fff;
    background-color: rgba(7, 17, 27, .5);
    overflow: hidden;
    .content-wrapper {
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;
      .avatar {
        display: inline-block;
        vertical-align: top;
        img {
          border-radius: 4px;
        }
      }
      .content {
        display: inline-block;
        margin-left: 16px;
        line-height: 1;
        .title {
            margin: 2px 0 8px;
          .brand{
            display: inline-block;
            width: 32px;
            height: 18px;
            margin-right: 6px;
            vertical-align: top;
            @include bg-image("brand")
            background-size: 32px 18px;
            background-repeat: no-repeat;
          }
          .name {
            display: inline-block;
            font-weight: 700;
            line-height: 18px;
            font-size: 16px;
          }
        }
        .description {
          margin-bottom: 10px;
          font-weight: 200;
          line-height: 12px;
          font-size: 12px;
        }
        .support {
          margin-bottom: 2px;
         
          .text {
            display: inline-block;
            margin-top: 2px;
            line-height: 12px;
            font-size:10px;
          }
        }
      }
      .supports-count {
        position: absolute;
        bottom: 18px;
        right: 12px;
        height: 24px;
        line-height: 24px;
        padding: 0px 8px;
        border-radius: 16px;
        background-color: rgba(0, 0, 0, .2);
        .count {
          display: inline-block;
          margin-top: 7px;
          font-size: 10px;
          line-height: 12px;
          font-weight: 200;
        }
        .icon-keyboard_arrow_right {
          display: inline-block;
          margin-left: 4px;
          line-height: 24px;
          font-size: 10px;
        }
      }
    }
    .bulletin-wrapper {
      position: relative;
      padding: 0 22px 0 12px;
      height: 28px;
      line-height: 28px;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      background-color: rgba(7, 17, 27, .2);
      .bulletin-title {
        display: inline-block;
        height: 12px;
        width: 22px;
        margin-top: 6px;
        vertical-align: top;
        @include bg-image('bulletin');
        background-size: 22px 12px;
      }
      .bulletin-text {
        vertical-align: top;
        margin: 0 4px;
        font-size: 10px;
      }
      .icon-keyboard_arrow_right {
        position: absolute;
        top: 7px;
        right: 12px;
      }
    }
    .background {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
    }
    .detail {
      position: fixed;
      top: 0;
      left: 0;
      height: 100%;
      width: 100%;
      z-index: 100;
      overflow: auto;
      background-color: rgba(7, 17, 27, .8);
      .detail-wrapper {
        width: 100%;
        min-height: 100%;
        .detail-main {
          padding: 64px 0;
          .name {
            line-height: 16px;
            font-weight: 700;
            font-size: 16px;
            text-align: center;
          }
          .star-wrapper {
            margin: 16px 0 28px;
            text-align: center;
          }
          .supports {
            width: 80%;
            margin: 0 auto 28px auto;
            .support-item {
              padding: 0 12px; 
              font-size: 0;
              margin-bottom: 12px;
              &:last-child {
                margin-bottom: 0;
              }
              .text {
                display: inline-block;
                font-size: 12px;
                line-height: 16px;
              }
            }
          }
          .bulletin {
            width: 80%;
            margin: 0 auto;
            .content {
              padding: 0 12px;
              font-size: 12px;
              line-height: 24px;
            }
          }
        }
      }
      .detail-close {
        position: relative;
        height: 32px;
        width: 32px;
        margin: -64px auto 0 auto;
        clear: both;
        font-size: 32px;
      }
    }
  }
</style>