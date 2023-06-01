<template>
  <div class="container">
    <img src="./assets/4697260.png" class="logo" />
    <Todolist/>
  </div>
</template>

<script>
import Todolist from './components/Todolist.vue'

export default {
  name: 'App',
  components: {
    Todolist
  }
}

</script>

<style>

.container{
  max-width: 400px;
  margin: 0 auto;
  width: 100vw;
  height: 100vh;
}

.logo {
  display: block;
  margin: 25px auto;
  height: 100px;
}

#app {
  font-family: 'Avenir', Arial, Helvetica, sans-serif;
  font-size: 24px;
}

</style>
