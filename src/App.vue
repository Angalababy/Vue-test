<template>
  <div id="app">
    <div class="container">
      <Search/>
      <Main/>
    </div>
  </div>
</template>
<script>
import Search from './components/Search.vue'
import Main from './components/Main.vue'
export default{
 components:{
  Search,
  Main
}
}

</script>
<style>


</style>
