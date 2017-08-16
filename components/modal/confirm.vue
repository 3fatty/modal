<!-- 确认操作弹出窗口
  @title
  @content
  @btn_cancel 左边取消按钮文字
  @btn_confirm 右边确认按钮文字
  @done: (tof) -> 点击按钮后的回调，点左边取消返回 false, 点右边确认返回 true

  # auhtor: bill
 -->
<style lang="less" type="text/css">
  @import '~assets/css/main.less';
  #dialog-confirm {
    background: rgba(0,0,0,0.5) !important;
    .innerWrapper { min-width:250px; max-width:300px; text-align:center; }
    .body { text-align:center; padding-bottom:5px; }
    .footer {
      // display: flex;
      padding: 10px;
      .btn {
        // flex: 1;
      }
    }
  }
</style>

<template>
  <div id="dialog-confirm" class="dialog-item outerWrapper">
    <div class="innerWrapper">
      <h4 class="title">
        标题
        <i class="btn-close">x</i>
      </h4>
      <div class="body" v-html="content"></div>
      <div class='footer'>
        <a class='btn' href='#' @click.prevent='handleCancel'>{{btn_cancel}}</a>
        <a class='btn accent' href='#' @click.prevent='handleConfirm'>{{btn_confirm}}</a>
      </div>
      <slot name="countdown"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    props:['postData'],
    data() {
      return {
        btn_cancel :'取消',
        btn_confirm:'确认',
        ...this.postData,
      }
    },
    methods: {
      handleCancel () {
        this.done? this.done(false): _modal.close(this.key);
      },
      handleConfirm () {
        this.done? this.done(true): _modal.close(this.key);
      }
    }
  }
</script>
