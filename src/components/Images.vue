<template>
  <div class="img">
    <!-- :src ->v-bind -->
    <img :src="link" alt="">
  </div>
</template>
<script>
export default {
  props: {
    link:String
  }
}
</script>

<style lang="scss">
.img{
  width: 100vw;
  height:10rem;
  background-color: #999;
}
</style>