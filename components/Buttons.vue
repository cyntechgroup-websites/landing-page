<template>
  <div class="p-2 group relative flex flex-col md:flex-row items-center transition transform rounded mt-4 md:mt-8 mb-4">
    <nav class="text-[#1b3664] z-10 flex flex-col md:flex-row justify-around gap-12 p-1 transition-all duration-300 ease-in-out">
      <button
        v-for="button in buttons"
        :key="button.name"
        @mouseenter="handleHoverStart(button)"
        @mouseleave="handleHoverEnd(button)"
        @click="navigate(button.url)"
        class="flex items-center justify-center w-[200px] h-[200px] rounded-full border border-transparent transition-all duration-300 ease-in-out bg-white shadow-lg hover:bg-[#fdc70c] hover:scale-110 hover:border-4 hover:border-[#ffffff]"
      >
        <img :src="`/images/${button.currentIcon}`" alt="" class="w-[80%] h-[80%]" />
      </button>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const buttons = ref([
  {
    name: 'Cyntech Tanks',
    url: 'https://www.cyntechtanks.com/en',
    icon: 'cyntech-tanks-logo.svg',
    hoverIcon: 'cyntech-tanks-logo-hover.svg',
    currentIcon: 'cyntech-tanks-logo.svg',
    type: 'tanks',
  },
  {
    name: 'MotionSteel',
    url: 'https://www.motionsteel.com/en',
    icon: 'motion-steel-logo.svg',
    hoverIcon: 'motion-steel-logo-hover.svg',
    currentIcon: 'motion-steel-logo.svg',
    type: 'motionSteel',
  },
  {
    name: 'Cyntech Anchors',
    url: '#',
    icon: 'cyntech-anchors-logo.svg',
    hoverIcon: 'cyntech-anchors-logo-hover.svg',
    currentIcon: 'cyntech-anchors-logo.svg',
    type: 'anchors',
  },
]);

const emit = defineEmits(['startSlideShow', 'stopSlideShow', 'hoverStart', 'hoverEnd']);

const handleHoverStart = (button) => {
  button.currentIcon = button.hoverIcon;
  emit('startSlideShow', button.type);
  emit('hoverStart');
};

const handleHoverEnd = (button) => {
  button.currentIcon = button.icon;
  emit('stopSlideShow');
  emit('hoverEnd');
};

const navigate = (url) => {
  window.open(url, '_blank');
};

</script>
