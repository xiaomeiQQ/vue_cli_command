<template>
  <div class="about">
    <h1>About</h1>
    <Button type="info">Info</Button>
    <Carousel v-model="value" loop>
      <CarouselItem v-for="item in 3" :key="item">
        <div class="demo-carousel">
            <img :src="require(`@/assets/images/test_0${item}.jpg`)">
        </div>
      </CarouselItem>
      <!-- <CarouselItem>
          <div class="carousel">1</div>
      </CarouselItem>
      <CarouselItem>
          <div class="carouse2">2</div>
      </CarouselItem>
      <CarouselItem>
          <div class="carousel3">3</div>
      </CarouselItem>
      <CarouselItem>
          <div class="carouse4">4</div>
      </CarouselItem> -->
    </Carousel>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                value: 0
            }
        }
    }
</script>