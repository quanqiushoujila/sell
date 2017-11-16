<template>
  <div class="title">
    <div class="line"></div>
    <div class="text">{{name}}</div>
    <div class="line"></div>
  </div>
</template>

<script>
  export default {
    props: {
      name: {
        type: String
      }
    }
  }
</script>

<style lang="scss" scoped>
  .title {
    display: flex;
    width: 80%;
    margin: 0 36px 34px;
    .line {
      position: relative;
      flex: 1;
      border-bottom: 1px solid rgba(255, 255, 255, .2);
      top: -6px;
    }
    .text {
      padding: 0 12px;
      font-size: 12px;
      line-height: 12px;
    }
  }
</style>