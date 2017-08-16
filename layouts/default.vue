<template>
  <div id="page-root" :data-page="$route.name==='index'?'home':$route.name">
    <modal-view/>
    <nuxt/>
  </div>
</template>

<script>
import modalView from '../components/modal';
export default {
  components: {
    // modalView : require('../components/modal'),
    modalView: modalView
  }
}
</script>

<style>
* { margin:0; padding:0; font:inherit; color:inherit; }
html, body { height:100%; font:16px sans-serif; }
a { text-decoration:none; }
</style>
