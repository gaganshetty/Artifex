<template>
  <div id="app">
    <div id="nav">
      <NavigationBar />
    </div>
    <router-view/>
  </div>
</template>

<script>
import NavigationBar from '@/components/NavigationBar.vue'
import Home from '@/components/NavigationBar.vue'
export default {
  components:{
          NavigationBar,
          Home
}
}

</script>
<style lang="scss">

</style>
