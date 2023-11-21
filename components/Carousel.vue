<template>
  <div class="carousel">
    <div class="carousel-track" :style="{ transform: `translateX(-${currentIndex * 100}%)` }">
      <div class="carousel-item flex justify-center" v-for="(item, index) in items" :key="index">
        <picture>
          <source media="(min-width: 768px)" :srcset="item.largeImage">
          <source media="(max-width: 767px)" :srcset="item.smallImage">
          <img class=" md:w-auto" :src="item.defaultImage" :alt="item.altText">
        </picture>
      </div>
    </div>
    <div class="carousel-dots">
      <button class="dot" v-for="(item, index) in items" :key="index" :class="{ 'active': index === currentIndex }"
        @click="goToItem(index)">
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { SfButton } from '@storefront-ui/vue';

const currentIndex = ref(0);
const items = ref([
  {
    largeImage: '/images/large1.png',
    smallImage: '/images/small1.png',
    defaultImage: '/images/large1.png',
    altText: 'image 1'
  },
  {
    largeImage: '/images/large2.png',
    smallImage: '/images/small2.png',
    defaultImage: '/images/large2.png',
    altText: 'image 2'
  },
  {
    largeImage: '/images/large3.png',
    smallImage: '/images/small3.png',
    defaultImage: '/images/large3.png',
    altText: 'image 3'
  }
]);

let autoSlideInterval = null;

const startAutoSlide = () => {
  stopAutoSlide();
  autoSlideInterval = setInterval(() => {
    nextItem();
  }, 2000);
};

const stopAutoSlide = () => {
  if (autoSlideInterval) {
    clearInterval(autoSlideInterval);
    autoSlideInterval = null;
  }
};

onMounted(() => {
  startAutoSlide();
});

const nextItem = () => {
  currentIndex.value = (currentIndex.value + 1) % items.value.length;
};

const goToItem = (index) => {
  stopAutoSlide();
  currentIndex.value = index;
};

</script>

<style>
.carousel {
  overflow: hidden;
  position: relative;
}

.carousel-track {
  display: flex;
  transition: transform 0.5s ease;
  /* Smooth transition for sliding */
}

.carousel-item {
  flex: 0 0 100%;
  /* Each item takes full width of the carousel */
  /* Other styles for the item */
}

/* Enter and leave transitions */
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s ease;
}

.slide-enter,
.slide-leave-to {
  transform: translateX(100%);
  /* Start and end state for sliding */
}

.slide-enter-to,
.slide-leave {
  transform: translateX(0);
  /* End and start state for sliding */
}

.carousel-dots {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  justify-content: center;
}

.dot {
  height: 10px;
  width: 10px;
  border-radius: 50%;
  background-color: #fff;
  margin: 0 5px;
  cursor: pointer;
  border: none;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.dot.active {
  opacity: 1;
}
</style>


