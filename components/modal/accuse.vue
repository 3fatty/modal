
<style lang="less">
  @import '~assets/css/main.less';
  #dialog-accuse {
    .innerWrapper {
      width:500px;
    }
    h3 { margin-bottom:5px; }
    .reasons {
      margin-bottom:15px;
      .btn {
        margin:2px 10px 2px 0;
      }
    }
    textarea { width:100%; height:100px;  resize:none; border:1px solid #ddd; }
  }
</style>

<template>
  <div id="dialog-accuse" class="dialog-item outerWrapper">
    <div class="innerWrapper">
      <!---->
      <div class="titlebar" v-if="title">
        <h4 v-html="title"></h4>
        <i class="btn-close ion-ios-close"></i>
      </div>
      <div class="ghost-titlebar" v-else><i class="btn-close">X</i></div>
      <!---->
      <div class="body">
        <h3>选择举报原因</h3>
        <div class="reasons">
          <a href="javascript:void(0);" v-for="(r,i) in reasons" class="btn" :class="{ 'accent':r.value===reason }" @click="selectReason(i)">{{r.text}}</a>
        </div>
        <h3>理由说明</h3>
        <textarea class="hidePlaceholderWhenFocus" placeholder="简述举报理由" v-model.trim="content"></textarea>
        <a href="javascript:void(0)" class="btn block accent confirm" @click="confirm(reason, content||undefined)">提交举报</a>
      </div>
      <!---->
      <p class="tips countdown" v-if="countdown">{{countdown}}秒后自动关闭</p>
      <!---->
    </div>
  </div>
</template>

<script>
  // import utils from '~/api/utils';
  export default {
    props:['postData'],
    data() {
      return {
        //原因代码，默认为0
        //1=人身攻击等不友善内容
        //2=抄袭、不规范引用类内容
        //3=灌水等无意义内容
        //4=色情、政治等违反法律法规的内容
        //5=垃圾广告信息
        //6=泄露隐私、骚扰等侵权内容
        reasons   : [
          // { text:'人身攻击', value:1, },
          // { text:'抄袭',    value:2, },
          // { text:'灌水',    value:3, },
          // { text:'违规内容', value:4, },
          // { text:'垃圾广告', value:5, },
          // { text:'侵权',    value:6, },
          { text:'灌水等无意义信息',    value:1 },
          { text:'垃圾广告信息',       value:2 },
          { text:'色情\政治等违规信息', value:3 },
        ],

        reason    : undefined,
        title     : '举报原因',
        content   : '',
        countdown : 0,
        ...this.postData
      };
    },
    methods: {
      selectReason(idx) {
        this.reason = this.reasons[idx].value;
      },
      async confirm() {
        _dialog.close( this.key );
      }
    //   async confirm() {
    //     if (this.reason == 0) {
    //       return _dialog.msg('请选择举报原因');
    //     }
    //     let r = await utils.requestSync({
    //       'url' : '/question/accusation/submit',
    //       //举报的类型，1=问题 2=回答 3=问题评论 4=回答评论 5=顾问点评 6=顾问点评的评论
    //       'body': { type: this.accuseType, id:this.id, reason:this.reason, comment:this.content }
    //     });
    //     _dialog.close( this.key );
    //     if (r.result === 8) {
    //       _dialog.msg('你已经举报过了', 2);
    //     } else if ( r.result===0 ) {
    //       _dialog.msg('举报成功', 2);
    //     } else {
    //       _dialog.msg(r.message, 2);
    //     }
    //   }
    }
  }
</script>
