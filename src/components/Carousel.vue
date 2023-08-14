<script setup>
import {ref, onMounted, onBeforeUnmount, reactive} from 'vue';

const currentImage = ref(0);
const data = reactive({
  'images':[]
})
let interval;

const startCarousel = () => {
  interval = setInterval(() => {
    currentImage.value = (currentImage.value + 1) % data.images.length;
  }, 5000);
};

const stopCarousel = () => {
  clearInterval(interval);
};

onMounted(async () => {

  try {
    const response = await fetch('https://admin.artico.com.bd/api/v1/sliders');
    const jsonData = await response.json();
    data.images = jsonData;
  } catch (error) {
    console.error('Error fetching data:', error);
  }

  startCarousel();
});

onBeforeUnmount(() => {
  stopCarousel();
});


</script>

<template>

  <div class="slider">
    <h1>Task1:</h1>
    <div class="carousel">
      <div class="carousel-container">
        <transition name="fade" mode="out-in">
          <img style="height: 600px!important;width: 100%" :key="currentImage" :src="data?.images[currentImage]?.full_url" :alt="data?.images[currentImage]?.id" />
        </transition>
      </div>
    </div>
  </div>

</template>

<style scoped>

.carousel {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.carousel-container {
  max-width: 100%;
  max-height: 100%;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>