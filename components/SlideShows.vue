<template>
  <section
    class="min-h-screen relative flex flex-col items-center justify-center bg-[#1b3664] overflow-hidden"
  >
    <article
      :class="[
        'absolute inset-0 w-full h-full transition-opacity duration-300 ease-in-out bg-center bg-no-repeat bg-cover',
        backgroundClass,
        { 'blur-lg': !isSlideFullyOpen() && !isMobile }
      ]"
      :style="backgroundStyle"
    ></article>
    <article
      :class="[
        'absolute inset-0 w-full h-full bg-[#1b3664] transition-transform duration-500 ease-in-out transform',
        coverClass,
      ]"
    ></article>
    <div class="relative z-10 flex flex-col w-full items-center justify-center">
      <slot></slot>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, computed } from "vue";

const backgroundClass = ref("opacity-0");
const backgroundStyle = ref({});
const coverClass = ref("translate-x-0");
const currentImageIndex = ref(0);
const isAnimating = ref(false);
let intervalId = null;
let animationTimeout = null;

const isMobile = ref(false);

const checkMobile = () => {
  if (process.client) {
    isMobile.value = window.innerWidth < 768;
  }
};

const isSlideFullyOpen = () => {
  return coverClass.value === "translate-x-full";
};

const imageGalleries = {
  tanks: [
    "/images/cyntech-tanks-1.jpg",
    "/images/cyntech-tanks-2.png",
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

const combinedGalleries = Object.values(imageGalleries).flat();

const preloadImages = () => {
  combinedGalleries.forEach((imageUrl) => {
    const img = new Image();
    img.src = imageUrl;
  });
};

const startSlideShow = (business) => {
  clearTimeout(animationTimeout);
  clearInterval(intervalId);

  isAnimating.value = true;
  currentImageIndex.value = 0;

  if (isMobile.value) {
    backgroundStyle.value = {
      backgroundImage: `url(${combinedGalleries[currentImageIndex.value]})`,
    };
  } else {
    backgroundStyle.value = {
      backgroundImage: `url(${
        imageGalleries[business][currentImageIndex.value]
      })`,
    };
  }

  backgroundClass.value = "opacity-100";
  coverClass.value = "translate-x-0";

  animationTimeout = setTimeout(() => {
    coverClass.value = "translate-x-full";

    intervalId = setInterval(() => {
      currentImageIndex.value = isMobile.value
        ? (currentImageIndex.value + 1) % combinedGalleries.length
        : (currentImageIndex.value + 1) % imageGalleries[business].length;

      backgroundClass.value = "opacity-0";

      setTimeout(() => {
        if (isMobile.value) {
          backgroundStyle.value = {
            backgroundImage: `url(${combinedGalleries[currentImageIndex.value]})`,
          };
        } else {
          backgroundStyle.value = {
            backgroundImage: `url(${
              imageGalleries[business][currentImageIndex.value]
            })`,
          };
        }

        backgroundClass.value = "opacity-100";
      }, 300);
    }, 1500);

    isAnimating.value = false;
  }, 500);
};

const stopSlideShow = () => {
  clearTimeout(animationTimeout);
  clearInterval(intervalId);

  coverClass.value = "translate-x-0";
  backgroundClass.value = "opacity-0";

  isAnimating.value = false;
};

onMounted(() => {
  checkMobile();
  window.addEventListener('resize', checkMobile);
  preloadImages();
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', checkMobile);
  clearInterval(intervalId);
  clearTimeout(animationTimeout);
});

defineExpose({ startSlideShow, stopSlideShow });
</script>
