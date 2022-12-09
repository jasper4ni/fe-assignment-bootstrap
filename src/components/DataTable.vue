<script setup lang="ts">
import { ref, watch } from 'vue';
import moment from 'moment';

export interface Props {
  items: any[];
  columns: any[];
  loading: boolean;
  clickable: boolean;
}

const props = defineProps<Props>();

// Ref define
const sortingColumn = ref({
  sort: '',
  order: '',
});
const sortItems = ref<any[]>([]);
const emit = defineEmits(['handleClick']);

// Reset sorted item if refresh
watch(
  () => props.items.length,
  (length) => {
    if (!length) {
      sortItems.value = [];
    }
  }
);

// sorting function
const sorting = (field: string) => {
  // Copy to other for not dirty original data
  sortItems.value = [...props.items];

  let order = '';
  if (sortingColumn.value.sort != field) {
    order = 'asc';
  } else {
    if (sortingColumn.value.order === 'asc') order = 'desc';
    else if (sortingColumn.value.order === 'desc') order = '';
    else order = 'asc';
  }

  sortingColumn.value = order
    ? { sort: field, order }
    : {
        sort: '',
        order: '',
      };

  if (sortingColumn.value.order == '') {
    sortItems.value = [];
    return;
  }

  sortItems.value.sort((a, b) => {
    let itemA, itemB;
    // datetime sorting
    if (field == 'date') {
      itemA = moment(a['datetime']);
      itemB = moment(b['datetime']);
    }
    // string sorting
    else {
      itemA = a[field].toUpperCase();
      itemB = b[field].toUpperCase();
    }
    switch (sortingColumn.value.order) {
      case 'asc':
        if (itemA < itemB) return -1;
        if (itemA > itemB) return 1;
        break;
      case 'desc':
        if (itemA < itemB) return 1;
        if (itemA > itemB) return -1;
        break;
      case '':
        order = 'asc';
        break;
    }
    return 0;
  });
};
</script>

<template>
  <div class="overflow-x-auto">
    <div class="py-2 inline-block min-w-full px-2">
      <div class="overflow-hidden -m-2">
        <table
          class="table-auto min-w-full border-separate border-spacing-y-4 p-2"
        >
          <thead>
            <tr>
              <th
                v-for="column in columns"
                :key="column.name"
                scope="col"
                class="font-medium text-gray-400 px-6 align-middle"
                :class="[column.align ?? 'text-left']"
              >
                {{ column.name }}
                <button
                  v-if="column.sortable"
                  class="align-middle"
                  @click="sorting(column.field)"
                >
                  <mdicon
                    v-if="
                      column.field == sortingColumn.sort &&
                      sortingColumn.order == 'desc'
                    "
                    name="arrow-down-thick"
                    size="20"
                    class="inline-block"
                  />
                  <mdicon
                    v-else-if="
                      column.field == sortingColumn.sort &&
                      sortingColumn.order == 'asc'
                    "
                    name="arrow-up-thick"
                    size="20"
                    class="inline-block"
                  />
                  <mdicon
                    v-else
                    name="arrow-up-down-bold"
                    size="20"
                    class="inline-block"
                  />
                </button>
              </th>
            </tr>
          </thead>
          <tbody>
            <template v-if="sortItems.length">
              <tr
                v-for="(row, index) in sortItems"
                :key="'row_' + index"
                class="rounded shadow"
                :class="[!!clickable ? 'cursor-pointer' : '']"
                @click="clickable ? emit('handleClick', row) : ''"
              >
                <slot name="items" :item="row" :index="index">
                  <td
                    class="px-6 py-4 whitespace-nowrap text-gray-500"
                    v-for="(column, index) in columns"
                    :class="[column.align ?? 'text-left', column.classes]"
                    :key="'data_' + index"
                  >
                    {{ row[column.field] || 'N/A' }}
                  </td>
                </slot>
              </tr>
            </template>
            <template v-else>
              <tr
                v-for="(row, index) in items"
                class="rounded shadow"
                :class="[!!clickable ? 'cursor-pointer' : '']"
                @click="clickable ? emit('handleClick', row) : ''"
                :key="'data_' + index"
              >
                <slot name="items" :item="row" :index="index">
                  <td
                    class="px-6 py-4 whitespace-nowrap text-gray-500"
                    v-for="(column, index) in columns"
                    :class="[column.align ?? 'text-left', column.classes]"
                    :key="'data_' + index"
                  >
                    {{ row[column.field] || 'N/A' }}
                  </td>
                </slot>
              </tr>
            </template>
          </tbody>
        </table>

        <div
          class="rounded shadow text-gray-500 text-center flex items-center justify-center h-14 m-2 mb-4"
          v-if="loading"
        >
          <div class="text-xl">Loading...</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.shadow {
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
}
</style>
