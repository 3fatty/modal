<style lang="less" type="text/css">
  @import '~assets/css/main.less';
  @titlebarHeight:30px;
  #dialog-container {
    position:fixed;
    z-index:10000000;
    .dialog-item {
      z-index:2000;
      &.transparent {
        .fixedFull(1000, transparent);
        // background:transparent;
        .ghost-titlebar {
          color:#1A233C;
        }
      }
      &.fullScreen{
      .fixedFull;
      }
    }
  }
  .dialog-item {
    .innerWrapper {
      box-sizing:border-box;
      position:fixed;
      left:50%;
      top:50%;
      transform:translate3d(-50%,-50%,0);
      background: #fff;
      border:1px solid #ddd;
      border-radius:4px;
    }

  i.btn-close {
    position:absolute;
    top:0;
    right:0;
    z-index:30;
    width:@titlebarHeight;
    height:@titlebarHeight+2;
    line-height: @titlebarHeight+2;
    font-size:18px;
    text-align:center;
    cursor:pointer;
  }
  .titlebar {
    position:relative;
    z-index:20;
    margin:-1px;
    height:@titlebarHeight;
    line-height: @titlebarHeight;
    color:#fff;
    background-color:#c92808;
    border:1px solid #ddd;
    border-radius:4px;
    border-color:#c92808;
    h4 {
      padding:0 25px;
    }
  &.btn-close { color:#fff; }
  &.body { margin-top:-4px; }
  }
  .body {
    position:relative;
    z-index:10;
    padding:25px;
    text-align:justify;
  }
  .countdown {
    position:relative;
    z-index:25;
    margin-top:-15px;
    padding:5px;
    text-align:center;
    font-size:12px;
    color:999; }
  }

  .title { line-height:30px; background:#c92808; color:#fff; }
</style>

<template>
  <!--
    <div is="message" :postData="{title:'标题', content:'内容内容内容', countdown:3}"></div>
    <div is="message" :postData="{ content:'内容内容内容', countdown:3}"></div>
    <div is="dialog-accuse" :postData="{ title:'举报原因', content:'内容内容内容'}"></div>
    <div is="dialog-bonusAppeal" :postData="{}"></div>
    <div is="dialog-appointment" :postData="{}"></div>
    <div is="dialog-share" :postData="{a:1}"></div>
    <div is="dialog-auth"></div>
    <div is="dialog-question" :postData="{fullScreen:true}"></div>
    <div is="dialog-login"></div>
    <div is="dialog-answering" :postData="{editorOption:{}}"></div>
    <div is="dialog-follow"></div>
  -->
  <div id="dialog-container">
    <div
      v-for="(dialog,idx) in dialogs"
      :data-key="dialog.key"
      :is="'dialog-'+dialog.type"
      :postData="dialog"
      :class="{'fullScreen':dialog.fullScreen, 'transparent':dialog.transparent}"
      :style="{'z-index':1000+idx}"
      v-if="dialog"
    >
      <p class="tips countdown" slot="countdown" v-if="dialog.countdown">{{dialog.countdown}} 秒后自动关闭</p>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        dialogs:[],
      }
    },
    components: {
      'dialog-message'     : require('./message'),
      'dialog-loading'     : require('./loading'),
      'dialog-confirm'     : require('./confirm'),
      // 'dialog-tagEditor'   : require('./tagEditor'),
      // 'dialog-release'   : require('./release'),
      // 'dialog-share'       : require('./share'),
      'dialog-login'       : require('./login'),
      'dialog-accuse'      : require('./accuse'),
      // 'dialog-editorFullscreen' : require('./editorFullscreen'),
      // 'dialog-allot'       : require('./allot'),
      // 'dialog-inputz'      : require('./Input'),
      // 'dialog-unresolved'  : require('./Unresolved'),
      // 'dialog-bonusAppeal' : require('./BonusAppeal'),
      // 'dialog-appointment' : require('./Appointment'),
      // 'dialog-auth'        : require('./Authentication'),
      // 'dialog-question'    : require('./Question'),
      // 'dialog-preview'     : require('./Preview'),
      // 'dialog-answering'   : require('./Answering'),
      // 'dialog-emailLogin'  : require('./EmailLogin'),
      // 'dialog-explain'     : require('./Explain'),
      // 'dialog-pay'         : require('./pay'),
      // 'dialog-follow'    : require('./Follow'),
      // 'dialog-new-chip'    : require('./new_chip'),
      // 'dialog-chip-link'   : require('./chip_link')
    },
    mounted() {
      // 全局 dialog/modal
      window._dialog = window._modal = (function(vm) {
        var _container = document.querySelector('#dialog-container');
        var _option    = {};
        var _currdialog = null;
        var _component = [];

        // 获取随机字符串, 作为key
        function getRandomStr(len, chars) {
          chars = chars || 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890';
          len   = len   || chars.length;

          var i = 0, max = chars.length, str='';
          for (; i<len; i++) {
              str += chars[Math.floor(Math.random()*max)];
          }
          return str;
        };
        // 允许多弹窗共存, 根据 key 值获取对应弹窗
        function getDialog(key) {
          let idx     = -1;
          let _option = vm.dialogs.filter(function(d,i) {
            if (d.key===key) {
              idx = i;
              return true;
            }
          })[0];
          if ( idx === -1 ) throw 'can\'t find dialog with key:' + key;
          return { 'option':_option, idx:idx, key:key };
        }

        // 打开弹窗, 预埋了钩子 beforeOpen 和 afterOpen
        function open(option) {
          let key = getRandomStr(16);
          option = setDefaultValue(option);
          option.timer = -1;
          vm.dialogs.push( option );
          // option.beforeOpen && option.beforeOpen();
          if ( typeof option.countdown === 'number' ) timer.start(option);
          // option.afterOpen  && option.afterOpen();
          return option.key = key;
        };
        // 关闭弹窗, 预埋了钩子 beforeClose 和 afterClose
        function close(key) {
          (function(temp) {
            timer.stop(temp.option.t);
            temp.option.beforeClose &&  temp.option.beforeClose(option);
            vm.$set(vm.dialogs, temp.idx, false);
            vm.$nextTick(()=>{
              vm.dialogs.splice(temp.idx,1);
              temp.option.afterClose  && temp.option.afterClose(temp);
            });
          }( getDialog(key) ))
        };

        // 预设的常用方法, confirm
        function confirm(content, opts = {}) {
          return _dialog.open({
            'type': 'confirm',
            title: (opts.title || '提示'),
            content: content,
            btn_cancel: opts.btn_cancel,
            btn_confirm: opts.btn_confirm,
            done: opts.done
          });
        };
        // 预设的常用方法, message
        function openMsg(content, countdown) {
          if ( !content ) return '';
          return _dialog.open( {'type':'message', content:content, countdown:countdown||2, fullScreen:false} );
        };


        // 设置默认样式, 标题/全屏/半透背景
        function setDefaultValue(option) {
          return {
            title:'',
            content:'',
            countdown:'',
            transparent:false,
            fullScreen:false,
            ...option,
          }
        };

        // 计时器, 用以自动关闭弹窗
        var timer = (function() {
          return  {
            start: function(option) {
              (function(option) {
                if ( !option.countdown || option.countdown <= 0 ) return close(option.key);
                option.t = setTimeout(function() {
                  option.countdown -= 1;
                  vm.$set(vm.dialogs, getDialog(option.key).idx, option);
                  timer.start( option );
                }, 1000);
              }(option))
            },
            stop(t) {
              clearTimeout(t);
            }
          }
        }());

        // 初始化处理, 事件托管, 点击 半透层/.btn-close 按钮关闭当前弹窗
        function init() {
          _container.addEventListener('click', function(e) {
            let target    = e.target;
            let classList = target.classList;
            if (
              classList.contains('dialog-item') ||
              classList.contains('btn-close')
            ) {
              let wrapper   = (function(t) {
                while( t && !t.classList.contains('dialog-item') ) {
                  t = t.parentNode;
                }
                close( t.getAttribute('data-key') );
              } (target));
            }
          });
        };
        init();
        return {
          open : open,
          close: close,
          msg  : openMsg,
          confirm: confirm
        }
      }(this));
    }

  };
</script>
