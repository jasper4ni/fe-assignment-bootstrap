<script setup lang="ts">
import { ref } from 'vue';

export interface Props {
  title: string;
}

defineProps<Props>();

const isOpen = ref(false);

const open = () => {
  isOpen.value = true;
};
const close = () => {
  isOpen.value = false;
};

defineExpose({
  open,
  close,
});
</script>

<template>
  <Transition name="fade">
    <div
      v-show="isOpen"
      class="fixed inset-0 flex items-center justify-center bg-gray-700 bg-opacity-30"
    >
      <div
        class="max-w-2xl p-12 mx-4 bg-white rounded-md shadow-xl w-3/5 max-w-screen-md"
      >
        <div class="flex items-center justify-between">
          <h3 class="text-3xl font-bold">{{ title || 'Title' }}</h3>
          <mdicon
            @click="isOpen = false"
            name="close"
            size="30"
            class="inline-block text-gray-400 cursor-pointer flex items-center justify-center"
          />
        </div>
        <div class="my-8">
          <slot>
            <p class="mb-4 text-sm">This is deafult modal content</p>
          </slot>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
