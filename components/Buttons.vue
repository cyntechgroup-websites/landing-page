<template>
  <section
    class="p-2 group relative flex flex-col md:flex-row items-center transition transform rounded mt-4 md:mt-8 mb-4"
  >
    <nav
      class="text-[#1b3664] z-10 flex flex-col md:flex-row justify-around gap-12 p-1 transition-all duration-300 ease-in-out"
    >
      <button
        v-for="button in buttons"
        :key="button.name"
        @mouseenter="handleHoverStart(button)"
        @mouseleave="handleHoverEnd(button)"
        @click="navigate(button.url)"
        class="flex items-center justify-center w-[170px] h-[170px] rounded-full border border-transparent transition-all duration-300 ease-in-out bg-white shadow-lg md:hover:bg-[#fdc70c] md:hover:scale-110 md:hover:border-4 md:hover:border-[#ffffff]"
      >
        <img
          :src="`/images/${button.currentIcon}`"
          alt=""
          class="w-[80%] h-[80%]"
        />
      </button>
    </nav>
  </section>
</template>

<script setup>
import { ref } from "vue";

const buttons = ref([
  {
    name: "Cyntech Tanks",
    url: "https://cyntech-tanks.vercel.app/",
    icon: "cyntech-tanks-logo.svg",
    hoverIcon: "cyntech-tanks-logo-hover.svg",
    currentIcon: "cyntech-tanks-logo.svg",
    type: "tanks",
  },
  {
    name: "MotionSteel",
    url: "https://www.motionsteel.com/en",
    icon: "motion-steel-logo.svg",
    hoverIcon: "motion-steel-logo-hover.svg",
    currentIcon: "motion-steel-logo.svg",
    type: "motionSteel",
  },
  {
    name: "Cyntech Anchors",
    url: "#",
    icon: "cyntech-anchors-logo.svg",
    hoverIcon: "cyntech-anchors-logo-hover.svg",
    currentIcon: "cyntech-anchors-logo.svg",
    type: "anchors",
  },
]);

const emit = defineEmits([
  "startSlideShow",
  "stopSlideShow",
  "hoverStart",
  "hoverEnd",
]);

const handleHoverStart = (button) => {
  if (window.innerWidth >= 768) {
    button.currentIcon = button.hoverIcon;
    emit("startSlideShow", button.type);
    emit("hoverStart");
  }
};

const handleHoverEnd = (button) => {
  if (window.innerWidth >= 768) {
    button.currentIcon = button.icon;
    emit("stopSlideShow");
    emit("hoverEnd");
  }
};

const navigate = (url) => {
  window.open(url, "_blank");
};
</script>
