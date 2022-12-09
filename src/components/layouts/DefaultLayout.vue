<script setup lang="ts">
import { RouterLink } from 'vue-router';
</script>

<template>
  <header
    class="container h-28 flex items-center flex-col sm:flex-row sm:justify-between flex-wrap"
  >
    <RouterLink to="/" class="my-2 md:my-0 h-12 w-36">
      <img src="/images/Logo.png" class="h-full w-full" />
    </RouterLink>

    <div class="block flex items-center">
      <RouterLink
        to="/about"
        class="mr-8 rounded-md p-1 hover:text-white hover:bg-blue-400 active:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300"
      >
        <mdicon name="bell-outline" size="26" />
      </RouterLink>
      <RouterLink
        to="/"
        class="mr-8 rounded-md p-1 hover:text-white hover:bg-blue-400 active:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300"
      >
        <mdicon name="cog-outline" size="26" />
      </RouterLink>
      <RouterLink
        to="/"
        class="rounded-md p-1 hover:text-white hover:bg-blue-400 active:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300"
      >
        <mdicon name="exit-to-app" size="26" />
      </RouterLink>
    </div>
  </header>

  <slot></slot>

  <footer class="p-12"></footer>
</template>
