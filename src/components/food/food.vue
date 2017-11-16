<template>
  <transition name="movess" enter-active-class="animated slideInRight" leave-active-class="animated slideOutLeft">
    <div class="food" v-show="showFlag">111</div>
  </transition>
</template>

<script>
  export default{
    props: {
      food: {
        type: Object
      }
    },
    data () {
      return {
        showFlag: false
      }
    },
    methods: {
      show () {
        this.showFlag = true
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '../../common/scss/animate.scss';
  
  .food {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: #fff;
    z-index: 20;
  }
</style>