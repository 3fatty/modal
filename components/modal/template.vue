<style lang="less">
@import "~assets/css/main.less";
#dialog-container
#dialog- {

}
</style>

<template>
  <div id="dialog-" class="dialog-item outerWrapper">
    <div class="innerWrapper">
      <!---->
      <div class="titlebar" v-if="title">
        <h4 v-html="title"></h4>
        <i class="btn-close ion-ios-close"></i>
      </div>
      <div class="ghost-titlebar" v-else><i class="btn-close ion-ios-close">X</i></div>
      <!---->
      <div class="body" v-html="content"></div>
      <!---->
      <slot name="countdown"></slot>
      <!---->
    </div>
  </div>
</template>

<script>
  export default {
    props:['postData'],
    data() {
      return {
        title     : '',
        ...this.postData
      }
    }
  }
</script>
