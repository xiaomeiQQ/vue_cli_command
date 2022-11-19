<template>
  <div class="news">
    <Images link="https://fakeimg.pl/200x100/"/>
    <Images link="https://fakeimg.pl/150x100/"/>
    <h1>News</h1>
    <HelloWorld title="緊張喔"/>
  </div>
</template>
<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import Images from '@/components/Images.vue'

export default {
  name: 'NewsView',
  components: {
    // HelloWorld: HelloWorld,
    HelloWorld,
    Images,
  }
}
</script>
