<template>
  <div id="wrapper">
    <header-app></header-app>
    <div id="content-wrapper">
      <div class="content-title"> <h1>Data Mahasiswa 2021</h1></div>
      <content-app></content-app>
    </div>
  </div>
</template>

<script>
import Header from '../components/elements/header'
import Content from '../components/elements/content'
export default {
  components : {
    'header-app': Header,
    'content-app': Content,
  }
}
</script>
