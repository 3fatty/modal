<style lang="less">
  @import '~assets/css/main.less';
  #dialog-message {
    pointer-events:none;
    background:none;
    .innerWrapper { padding:0 5px; min-width:100px; max-width:300px; text-align:center;  background:rgba(0,0,0,0.6); border:0 none; box-shadow:0 0 5px #aaa; }
    .body { padding:10px; font-size:14px; text-align:center; color:#fff; border:0 none; border-radius:3px; }
    .countdown { margin-top:-10px; color:#ccc; font-size:10px; }
  }
</style>

<template>
  <div id="dialog-message" class="dialog-item outerWrapper">
    <div class="innerWrapper">
      <div class="body" v-html="content"></div>
      <slot name="countdown"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    props:['postData'],
    data() {
      return {
        ...this.postData ,
      }
    },
  }
</script>
