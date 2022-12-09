<script setup lang="ts">
import DefaultLayout from './components/layouts/DefaultLayout.vue';
import { RouterView } from 'vue-router';
import Banner from './components/layouts/Banner.vue';
import { watch } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();

watch(route, () => {
  document.title = `${String(route.name)} - Kiratech`;
});
</script>

<template>
  <DefaultLayout>
    <Banner />
    <RouterView />
  </DefaultLayout>
</template>
