<template>
  <div class="min-h-screen relative flex flex-col items-center justify-center bg-[#1b3664] overflow-hidden">
    <div 
      :class="['absolute inset-0 w-full h-full transition-transform duration-300 ease-in-out bg-center bg-no-repeat bg-cover', backgroundClass]" 
      :style="backgroundStyle"
    ></div>
    <div class="relative z-10 flex flex-col w-full items-center justify-center">
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const backgroundClass = ref('translate-x-full');
const backgroundStyle = ref({});
const currentImageIndex = ref(0);
let intervalId = null;
let isHovering = false;

const imageGalleries = {
  tanks: [
    "url('/images/cyntech-tanks-1.jpg')",
    "url('/images/cyntech-tanks-2.jpg')",
    "url('/images/cyntech-tanks-3.jpg')"
  ],
  motionSteel: [
    "url('/images/motion-steel-1.jpg')",
    "url('/images/motion-steel-2.jpg')",
    "url('/images/motion-steel-3.jpg')"
  ],
  anchors: [
    "url('/images/cyntech-anchors-1.jpg')",
    "url('/images/cyntech-anchors-2.jpg')",
    "url('/images/cyntech-anchors-3.jpg')"
  ]
};

const startSlideShow = (business) => {
  clearInterval(intervalId);
  if (isHovering) return;
  
  isHovering = true;
  currentImageIndex.value = 0;
  backgroundClass.value = 'translate-x-full';

  setTimeout(() => {
    backgroundStyle.value = { backgroundImage: imageGalleries[business][currentImageIndex.value] };
    backgroundClass.value = 'translate-x-0';

    intervalId = setInterval(() => {
      if (!isHovering) return;

      backgroundClass.value = 'translate-x-full';
      setTimeout(() => {
        currentImageIndex.value = (currentImageIndex.value + 1) % imageGalleries[business].length;
        backgroundStyle.value = { backgroundImage: imageGalleries[business][currentImageIndex.value] };
        backgroundClass.value = 'translate-x-0';
      }, 500);
    }, 2000);
  }, 300);
};

const stopSlideShow = () => {
  isHovering = false;
  clearInterval(intervalId);
  backgroundClass.value = 'translate-x-full';

  setTimeout(() => {
    backgroundStyle.value = { backgroundColor: '#1b3664' };
    backgroundClass.value = 'translate-x-0';
  }, 300);
};

defineExpose({ startSlideShow, stopSlideShow });
</script>
