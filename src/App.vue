<template>

  <Main msg="Hello Vue 3.0 + Vite" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Main from './components/Main.vue'
export default {
  name: 'App',
  components: {
    Main
  }
}
</script>
