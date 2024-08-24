<template>
  <Welcome />
  <SlideShows ref="slideShow">
    <div
      :class="['relative w-full flex justify-center items-center transition-all duration-300', isHovered ? 'bg-white' : '']"
    >
      <img
        :src="logoSrc"
        alt="Cyntech Group Logo"
        class="w-[800px] my-4 md:px-12 px-4 transition-all duration-300"
      />
    </div>
    <Buttons
      @startSlideShow="handleStartSlideShow"
      @stopSlideShow="handleStopSlideShow"
      @hoverStart="handleHoverStart"
      @hoverEnd="handleHoverEnd"
    />
  </SlideShows>
</template>

<script setup>
import { ref } from 'vue';

const slideShow = ref(null);
const isHovered = ref(false);
const logoSrc = ref('/images/cyntech-group-logo.svg');

const handleStartSlideShow = (business) => {
  slideShow.value.startSlideShow(business);
};

const handleStopSlideShow = () => {
  slideShow.value.stopSlideShow();
};

const handleHoverStart = () => {
  isHovered.value = true;
  logoSrc.value = '/images/cyntech-group-logo-rotate.svg';
};

const handleHoverEnd = () => {
  isHovered.value = false;
  logoSrc.value = '/images/cyntech-group-logo.svg';
};
</script>
