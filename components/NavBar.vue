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
