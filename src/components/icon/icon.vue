<template>
  <span :class="[classMap[type], icon]"></span>
</template>

<script>
  export default {
    props: {
      type: {
        type: Number
      },
      size: {
        type: Number
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      this.icon = 'icon-' + this.size
    }
  }
</script>

<style lang="scss" scoped>
@import '../../common/scss/mixin.scss';
  .icon-1 {
    display: inline-block;
    width: 12px;
    height: 12px; 
    margin-right: 4px;
    vertical-align: top;
    background-repeat: no-repeat;
    background-size: 12px 12px;
    &.decrease {
      @include bg-image("decrease_1");
    }
    &.discount {
      @include bg-image("discount_1");
    }
    &.guarantee {
      @include bg-image("guarantee_1");
    }
    &.invoice {
      @include bg-image("invoice_1");
    }
    &.special {
      @include bg-image("special_1");
    }
  }
  .icon-2 {
      display: inline-block;
      width: 16px;
      height: 16px;
      margin-right: 6px;
      vertical-align: top;
      background-repeat: no-repeat;
      background-size: 16px 16px;
      &.decrease {
        @include bg-image("decrease_2");
      }
      &.discount {
        @include bg-image("discount_2");
      }
      &.guarantee {
        @include bg-image("guarantee_2");
      }
      &.invoice {
        @include bg-image("invoice_2");
      }
      &.special {
        @include bg-image("special_2");
      }
    }
    .icon-3 {
      display: inline-block;
      width: 12px;
      height: 12px;
      margin-right: 2px;
      vertical-align: top;
      background-repeat: no-repeat;
      background-size: 12px 12px;
      &.decrease {
        @include bg-image("decrease_3");
      }
      &.discount {
        @include bg-image("discount_3");
      }
      &.guarantee {
        @include bg-image("guarantee_3");
      }
      &.invoice {
        @include bg-image("invoice_3");
      }
      &.special {
        @include bg-image("special_3");
      }
    }
</style>