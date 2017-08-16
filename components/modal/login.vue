<style lang="less">
  @import '~assets/css/main.less';
  #dialog-login {
    background-color:@maskBgColor;
    .innerWrapper{
      position: fixed;
      left: 50%;
      top: 50%;
      padding: 15px;
      text-align: center;
      background: #f7fafc;
      transform: translate3d(-50%,-50%,0);
    }
    .body { overflow:hidden; }
  }

</style>

<template>
  <div id="dialog-login" class="dialog-item outerWrapper fullScreen">
    <div class="innerWrapper">
      <i class="btn-close">X</i>
      <iframe :src='src' frameborder="0" height="420"></iframe>
    </div>
  </div>
</template>

<script>

  // import config from '~/env.config';
  export default {
    props:['postData'],
    data() {
      return {
        title : false,
        src   : '',
        ...this.postData,
      }
    },
    beforeMount() {
      // let uri = 'http://www.pengpengkeng.com/wx/oauth2/callback-dev' + `?redirect=${location.pathname}`;
      // this.src =  'https://open.weixin.qq.com/connect/qrconnect?appid=wxefce8ecb0fdd23c9&redirect_uri='
      //   + 'http://www.pengpengkeng.com/wx/oauth2/callback-dev' + `?redirect=${location.pathname}`
      //   + '&response_type=code&scope=snsapi_login&state=123456#wechat_redirect';

    }
  }
</script>
