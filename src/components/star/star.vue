<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" class="star-item" :class="itemClass"></span>
  </div>
</template>

<script>
  const LENGTH = 5
  const CLS_ON = 'on'
  const CLS_HALF = 'half'
  const CLS_OFF = 'off'
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType () {
        return 'star-' + this.size
      },
      itemClasses () {
        let result = []
        let score = Math.floor(this.score * 2) / 2
        let hasDecimal = this.score % 1 !== 0
        let integer = Math.floor(score)
        for (let i = 0; i < integer; i++) {
          result.push(CLS_ON)
        }
        if (hasDecimal) {
          result.push(CLS_HALF)
        }
        while (result.length < LENGTH) {
          result.push(CLS_OFF)
        }
        return result
      }
    }
  }
</script>

<style lang="scss" scoped>
@import '../../common/scss/mixin.scss';

  .star {
    .star-item {
      display: inline-block;
      background-repeat: no-repeat;
    }
    &.star-24 {
      .star-item {
        margin-right: 2px;
        width: 10px;
        height: 10px;
        background-size: 10px 10px;
        &:last-child {
          margin-right: 0;
        }
        &.on {
          @include bg-image('../../common/img/star24_on')
        }
        &.off {
          @include bg-image('../../common/img/star24_off')
        }
        &.half {
          @include bg-image('../../common/img/star24_half')
        }
      }
    }
    &.star-36 {
      .star-item {
        margin-right: 6px;
        width: 15px;
        height: 15px;
        background-size: 15px 15px;
        &:last-child {
          margin-right: 0;
        }
        &.on {
          @include bg-image('../../common/img/star36_on')
        }
        &.off {
          @include bg-image('../../common/img/star36_off')
        }
        &.half {
          @include bg-image('../../common/img/star36_half')
        }
      }
    }
    &.star-48 {
      .star-item {
        margin-right: 22px;
        width: 20px;
        height: 20px;
        background-size: 20px 20px;
        &:last-child {
          margin-right: 0;
        }
        &.on {
          @include bg-image('../../common/img/star48_on')
        }
        &.off {
          @include bg-image('../../common/img/star48_off')
        }
        &.half {
          @include bg-image('../../common/img/star48_half')
        }
      }
    }
    
  }
</style>