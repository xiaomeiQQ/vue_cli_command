<template>
  <div class="hello">
    <img alt="Vue logo" src="../assets/logo.png">
    <div class="img">
      <img src="" alt="">
      <img :src="require(`@/assets/images/fortest.jpg`)">
    </div>
    <!-- <div :style="{
      backgroundImage:`url(${require('@/assets/images')})`
      }"></div> -->
      <p>{{ title }}</p>
      <h1>{{ msg }}</h1>
      <button>搶票喔</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    title: String,
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
