<template>
  <div>
    
    
    <RouterView />
  </div>
</template>
<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Prompt:wght@200;300;400;500;600;700;800;900&display=swap");
#app {
  font-family: Prompt, Helvetica, Arial, sans-serif;
}
</style>
