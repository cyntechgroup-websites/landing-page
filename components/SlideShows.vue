<template>
  <div
    class="min-h-screen relative flex flex-col items-center justify-center bg-[#1b3664] overflow-hidden"
  >
    <div
      :class="[
        'absolute inset-0 w-full h-full transition-opacity duration-300 ease-in-out bg-center bg-no-repeat bg-cover',
        backgroundClass,
      ]"
      :style="backgroundStyle"
    ></div>
    <div
      :class="[
        'absolute inset-0 w-full h-full bg-[#1b3664] transition-transform duration-500 ease-in-out transform',
        coverClass,
      ]"
    ></div>
    <div class="relative z-10 flex flex-col w-full items-center justify-center">
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

const backgroundClass = ref("opacity-0");
const backgroundStyle = ref({});
const coverClass = ref("translate-x-0");
const currentImageIndex = ref(0);
let intervalId = null;
let animationTimeout = null;
let isAnimating = false;

const imageGalleries = {
  tanks: [
    "/images/cyntech-tanks-1.jpg",
    "/images/cyntech-tanks-2.jpg",
    "/images/cyntech-tanks-3.jpg",
  ],
  motionSteel: [
    "/images/motion-steel-1.jpg",
    "/images/motion-steel-2.jpg",
    "/images/motion-steel-3.jpg",
  ],
  anchors: [
    "/images/cyntech-anchors-1.jpg",
    "/images/cyntech-anchors-2.jpg",
    "/images/cyntech-anchors-3.jpg",
  ],
};

const preloadImages = () => {
  Object.values(imageGalleries)
    .flat()
    .forEach((imageUrl) => {
      const img = new Image();
      img.src = imageUrl;
    });
};

const startSlideShow = (business) => {
  clearTimeout(animationTimeout);
  clearInterval(intervalId);

  isAnimating = true;
  currentImageIndex.value = 0;

  backgroundStyle.value = {
    backgroundImage: `url(${
      imageGalleries[business][currentImageIndex.value]
    })`,
  };
  backgroundClass.value = "opacity-100";
  coverClass.value = "translate-x-0";

  animationTimeout = setTimeout(() => {
    coverClass.value = "translate-x-full";

    intervalId = setInterval(() => {
      currentImageIndex.value =
        (currentImageIndex.value + 1) % imageGalleries[business].length;
      backgroundClass.value = "opacity-0";

      setTimeout(() => {
        backgroundStyle.value = {
          backgroundImage: `url(${
            imageGalleries[business][currentImageIndex.value]
          })`,
        };
        backgroundClass.value = "opacity-100";
      }, 300);
    }, 2500);

    isAnimating = false;
  }, 500);
};

const stopSlideShow = () => {
  clearTimeout(animationTimeout);
  clearInterval(intervalId);

  coverClass.value = "translate-x-0";
  backgroundClass.value = "opacity-0";

  isAnimating = false;
};

onMounted(() => {
  preloadImages();
});

onBeforeUnmount(() => {
  clearInterval(intervalId);
  clearTimeout(animationTimeout);
});

defineExpose({ startSlideShow, stopSlideShow });
</script>
