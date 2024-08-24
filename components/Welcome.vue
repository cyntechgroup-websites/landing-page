<template>
  <main
    class="flex flex-col items-center justify-center h-screen text-[#fdc70c]"
  >
    <header class="text-center">
      <h1 class="text-6xl font-bold mb-8">
        Welcome to
        <span class="inline-block pr-2 text-[#1b3664] cursor-blink">
          {{ currentWord }}<span class="invisible">|</span>
        </span>
      </h1>
    </header>
    <nav
      ref="arrow"
      class="mt-12 animate-bounce cursor-pointer transition-all duration-150 md:hover:text-[#1b3664]"
      @click="scrollToBottom"
      @mouseover="handleMouseOver"
      @mouseleave="handleMouseLeave"
    >
      <svg
        class="w-10 h-10"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M19 9l-7 7-7-7"
        ></path>
      </svg>
    </nav>
  </main>
</template>

<script setup>
import { ref } from "vue";

const words = ["Excellence", "Dedication", "Expertise", "CYNTECH"];
const currentWord = ref(words[0]);
const arrow = ref(null);

let wordIndex = 0;
let typingSpeed = 150;
let erasingSpeed = 100;
let delayBetweenWords = 1000;

const typeWord = async () => {
  const word = words[wordIndex];
  for (let i = 1; i <= word.length; i++) {
    currentWord.value = word.substring(0, i);
    await new Promise((resolve) => setTimeout(resolve, typingSpeed));
  }
  await new Promise((resolve) => setTimeout(resolve, delayBetweenWords));
  eraseWord();
};

const eraseWord = async () => {
  const word = currentWord.value;
  for (let i = word.length; i >= 0; i--) {
    currentWord.value = word.substring(0, i);
    await new Promise((resolve) => setTimeout(resolve, erasingSpeed));
  }
  wordIndex = (wordIndex + 1) % words.length;
  await new Promise((resolve) => setTimeout(resolve, delayBetweenWords));
  typeWord();
};

const scrollToBottom = () => {
  const slideShowsSection = document.querySelector("section.min-h-screen");
  if (slideShowsSection) {
    const slideShowsPosition =
      slideShowsSection.getBoundingClientRect().top + window.scrollY;
    window.scrollTo({
      top: slideShowsPosition,
      behavior: "smooth",
    });
  }
};


const handleMouseOver = () => {
  if (window.innerWidth >= 768) {
    pauseBounce();
  }
};

const handleMouseLeave = () => {
  if (window.innerWidth >= 768) {
    resumeBounce();
  }
};

const pauseBounce = () => {
  arrow.value.style.animationPlayState = "paused";
  arrow.value.classList.add("text-[#1b3664]");
};

const resumeBounce = () => {
  arrow.value.style.animationPlayState = "running";
  arrow.value.classList.remove("text-[#1b3664]");
};

onMounted(() => {
  typeWord();
});
</script>
