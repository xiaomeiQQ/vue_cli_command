<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld title="中午12:00" msg="Welcome to Your Vue.js App"/>
    <HelloWorld title="Mamamoo" msg="1"/>
    <HelloWorld title="韓國女團" msg="2"/>
    <HelloWorld title="演唱會" msg="3"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}
</script>
