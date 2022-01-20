<template>
  <div id="app">
    <BaseForm/>
  </div>
</template>

<script>
  import BaseForm from './components/BaseForm.vue'

  export default {
    name: 'App',
    components: {
      BaseForm
    }
  }
</script>

<style>
  #app {
    font-family: Arial, Helvetica, sans-serif;
    margin-top: 30px;
    background: #fafafa;
  }
</style>
