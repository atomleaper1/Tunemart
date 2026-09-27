```vue
<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted, watch } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();

const open = ref(false);
const y = ref(0);

const solidNav = computed(
  () => y.value > 40 || !["/", "/about"].includes(route.path),
);

const toggle = () => (open.value = !open.value);
const close = () => (open.value = false);

const onScroll = () => (y.value = window.scrollY);

onMounted(() => {
  window.addEventListener("scroll", onScroll, { passive: true });
});

onUnmounted(() => {
  window.removeEventListener("scroll", onScroll);
});

watch(() => route.path, close);
</script>

<template>
  <!-- Navbar -->
  <header
    class="fixed top-0 left-0 w-full h-[var(--header-height)] z-[1000] transition-all duration-300 flex items-center text-white"
    :class="
      solidNav
        ? 'bg-black/75 backdrop-blur-md shadow-[0_4px_20px_rgba(0,0,0,0.15)]'
        : 'bg-transparent'
    "
  >
    <div
      class="container flex justify-between md:grid md:grid-cols-[1fr_auto_1fr] items-center w-full"
    >
      <!-- Left Navigation -->
      <div class="hidden md:flex gap-8 justify-start">
        <NuxtLink
          to="/"
          class="font-medium text-white uppercase text-base opacity-90 tracking-wide transition-opacity hover:opacity-100"
        >
          Store
        </NuxtLink>

        <NuxtLink
          to="/products"
          class="font-medium text-white uppercase text-base opacity-90 tracking-wide transition-opacity hover:opacity-100"
        >
          Products
        </NuxtLink>
      </div>

      <!-- Center Logo -->
      <NuxtLink
        to="/"
        class="flex items-center justify-center gap-3 group no-underline"
      >
        <!-- Logo Image -->
        <div
          class="w-10 h-10 flex items-center justify-center rounded-lg bg-white/10 p-2 transition-all duration-300 group-hover:bg-white/20"
        >
          <img
            src="/khu.png"
            alt="TuneMart logo"
            class="w-full h-full object-contain transition-transform duration-300 group-hover:rotate-6"
          />
        </div>

        <!-- Logo Text -->
        <span
          class="text-2xl font-bold tracking-tight text-white transition-opacity duration-300 group-hover:opacity-80"
        >
          TuneMart
        </span>
      </NuxtLink>

      <!-- Right Navigation -->
      <div class="flex gap-8 justify-end items-center">
        <NuxtLink
          to="/about"
          class="hidden md:block font-medium text-white uppercase text-base opacity-90 tracking-wide transition-opacity hover:opacity-100"
        >
          About Us
        </NuxtLink>

        <NuxtLink
          to="/auth"
          class="hidden md:block font-medium text-white uppercase text-base opacity-90 tracking-wide transition-opacity hover:opacity-100"
        >
          Login / Register
        </NuxtLink>

        <!-- Mobile Menu Button -->
        <button
          class="block md:hidden text-2xl bg-transparent text-white border-none cursor-pointer"
          @click="toggle"
          aria-label="Toggle menu"
        >
          <Icon name="ri:menu-4-line" size="24" />
        </button>
      </div>
    </div>
  </header>

  <!-- Mobile Sidebar -->
  <aside
    class="fixed top-0 right-0 w-[300px] h-screen bg-[#1a1a1a] z-[2000] transition-transform duration-300 p-8 flex flex-col gap-8 shadow-[-5px_0_15px_rgba(0,0,0,0.5)]"
    :class="open ? 'translate-x-0' : 'translate-x-full'"
  >
    <!-- Mobile Header -->
    <div class="flex items-center justify-between">
      <NuxtLink
        to="/"
        class="flex items-center gap-3 no-underline"
        @click="close"
      >
        <div
          class="w-9 h-9 flex items-center justify-center rounded-lg bg-white/10 p-1.5"
        >
          <img
            src="/khu.png"
            alt="TuneMart logo"
            class="w-full h-full object-contain"
          />
        </div>

        <span class="text-xl font-bold text-white">
          TuneMart
        </span>
      </NuxtLink>

      <button
        class="bg-transparent border-none text-white cursor-pointer"
        @click="close"
        aria-label="Close menu"
      >
        <Icon name="ri:close-line" size="26" />
      </button>
    </div>

    <!-- Mobile Links -->
    <div class="flex flex-col gap-6 mt-4">
      <NuxtLink
        to="/"
        class="text-white no-underline text-xl uppercase tracking-widest transition-colors hover:text-primary"
        @click="close"
      >
        Store
      </NuxtLink>

      <NuxtLink
        to="/products"
        class="text-white no-underline text-xl uppercase tracking-widest transition-colors hover:text-primary"
        @click="close"
      >
        Products
      </NuxtLink>

      <NuxtLink
        to="/about"
        class="text-white no-underline text-xl uppercase tracking-widest transition-colors hover:text-primary"
        @click="close"
      >
        About Us
      </NuxtLink>

      <NuxtLink
        to="/auth"
        class="text-white no-underline text-xl uppercase tracking-widest transition-colors hover:text-primary"
        @click="close"
      >
        Login / Register
      </NuxtLink>
    </div>
  </aside>

  <!-- Backdrop -->
  <div
    v-if="open"
    class="fixed inset-0 bg-black/50 z-[1500]"
    @click="close"
  ></div>
</template>
```
