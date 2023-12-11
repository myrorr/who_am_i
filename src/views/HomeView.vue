<script setup lang="ts">
import Carousel from '../components/Carousel.vue';
import Slide from '../components/Slide.vue';

const carouselSlides : string[] = ['fuji_cuddle', 'fuji_sit', 'fuji_upside_down']

</script>

<template>
  <main>
  <div class="home">
    <Carousel class="relative max-h-screen h-screen" v-slot="{currentSlide}">
      <Slide v-for="(slide, index) in carouselSlides" :key="index">
        <div v-show="currentSlide === index + 1" class="absolute top-0 left-0 w-full max-h-full h-full">
          <img :src="`/${slide}.jpg`" :id="`img-${index + 1}`" class="w-full h-full object-contain">
        </div>
      </Slide>
    </Carousel>
  </div>
  </main>
</template>

<style scoped>


</style>
