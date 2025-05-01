<template>
  <nav
      class="fixed top-0 left-0 w-full transition-colors duration-300 z-50"
      :class="{ 'bg-gradient-to-br from-gray-900 to-gray-800 backdrop-blur-md shadow': isScrolled, 'bg-transparent': !isScrolled }"
  >
    <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
      <!-- Logo -->
      <a href="/" class="flex items-center space-x-3 rtl:space-x-reverse">
        <img src="/logoelektro.png" class="h-8" alt="Logo" />
        <span class="self-center text-2xl text-amber-400 font-semibold whitespace-nowrap text-white">Arial Project</span>
      </a>

      <!-- Mobile Button -->
      <button
          @click="drawer"
          type="button"
          class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-white rounded-lg md:hidden hover:bg-gray-100 hover:text-black focus:outline-none focus:ring-2 focus:ring-gray-200"
      >
        <svg
            class="w-5 h-5"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 17 14"
        >
          <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M1 1h15M1 7h15M1 13h15"
          />
        </svg>
      </button>

      <!-- Desktop Nav -->
      <div class="hidden md:block">
        <ul class="flex space-x-8 text-sm font-sans text-white">
          <li><a href="/">Home</a></li>
          <li><a href="/about-us">About Us</a></li>
          <li><a href="/info">Info</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </div>

      <!-- Overlay -->
      <transition
          enter-class="opacity-0"
          enter-active-class="ease-out transition-medium"
          enter-to-class="opacity-100"
          leave-class="opacity-100"
          leave-active-class="ease-out transition-medium"
          leave-to-class="opacity-0"
      >
        <div
            v-show="isOpen"
            @keydown.esc="isOpen = false"
            class="z-10 fixed inset-0 transition-opacity"
        >
          <div
              @click="isOpen = false"
              class="absolute inset-0 bg-black opacity-50"
              tabindex="0"
          ></div>
        </div>
      </transition>

      <!-- Drawer Menu -->
      <aside
          class="p-5 transform top-0 left-0 w-64 bg-white fixed h-full overflow-auto ease-in-out transition-all duration-300 z-30"
          :class="isOpen ? 'translate-x-0' : '-translate-x-full'"
      >
        <button
            class="absolute top-0 right-0 mt-4 mr-4"
            @click="isOpen = false"
        >
          <svg
              class="w-6 h-6"
              fill="none"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              viewBox="0 0 24 24"
              stroke="currentColor"
          >
            <path d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>

        <span
            @click="isOpen = false"
            class="flex w-full items-center p-4 border-b"
        >
          <p>Arial Project</p>
        </span>

        <ul class="divide-y font-sans">
          <li><a href="/" @click="isOpen = false" class="my-4 inline-block">Home</a></li>
          <li><a href="/about-us" @click="isOpen = false" class="my-4 inline-block">About Us</a></li>
          <li><a href="/info" @click="isOpen = false" class="my-4 inline-block">Info</a></li>
          <li><a href="/contact" @click="isOpen = false" class="my-4 inline-block">Contact</a></li>
        </ul>
      </aside>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      isOpen: false,
      isScrolled: false,
    };
  },
  methods: {
    drawer() {
      this.isOpen = !this.isOpen;
    },
  },
  watch: {
    isOpen: {
      immediate: true,
      handler(isOpen) {
        if (process.client) {
          if (isOpen) document.body.style.setProperty("overflow", "hidden");
          else document.body.style.removeProperty("overflow");
        }
      },
    },
  },
  mounted() {
    document.addEventListener("keydown", (e) => {
      if (e.keyCode === 27 && this.isOpen) this.isOpen = false;
    });

    window.addEventListener("scroll", () => {
      this.isScrolled = window.scrollY > 10;
    });
  },
};
</script>

<style scoped>
aside {
  will-change: transform;
}
</style>
