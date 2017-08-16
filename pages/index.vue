<style lang="less">
.container {
  padding:30px;
}
</style>
<template>
  <section class="container">
    <a class="btn" @click="msg">文字提示框, 带自动关闭</a>
    <a class="btn" @click="loading">{{!!this.isLoading? '关闭':'打开'}}loading</a>
    <a class="btn" @click="confirm">选择回话框</a>
    <br>
    <br>
    <br>
    <a class="btn" @click="iframe('https://www.baidu.com/s?wd=hello')">打开 iframe</a>
    <a class="btn" @click="accuse">自定义会话框</a>
  </section>
</template>

<script>
export default {
  data() {
    return {
      isLoading:'',
    };
  },
  methods: {
    msg() {
      _modal.open({type:'message', content:'文字提醒', countdown:3})
    },
    loading() {
      this.isLoading =
        this.isLoading?
        _modal.close(this.isLoading):
        _modal.open({type:'loading'}), '';
    },
    confirm() {
      _modal.open({type:'confirm', content:'文字描述'})
    },
    iframe(src) {
      _modal.open({type:'login', src})
    },
    accuse() {
      _modal.open({type:'accuse', fullScreen:true})
    }
  }
}
</script>
