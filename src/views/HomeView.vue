<script setup lang="ts">
import { ref, onMounted } from 'vue';
import DataTable from '@/components/DataTable.vue';
import moment from 'moment';
import Modal from '@/components/Modal.vue';

export interface ModalType {
  name: string;
  date: string;
  gender: string;
  country: string;
  email: string;
}

// Ref define
const users = ref([]);
const columns = ref([
  { name: 'Date', field: 'date', sortable: true },
  { name: 'Name', field: 'name', classes: ' font-bold', sortable: true },
  { name: 'Gender', field: 'gender', classes: ' capitalize', sortable: true },
  { name: 'Country', field: 'country', classes: ' font-bold', sortable: true },
  { name: 'Email', field: 'email', align: 'text-right', sortable: true },
]);
const loading = ref(true);
const RefModal = ref<InstanceType<typeof Modal> | null>(null);
const modalData = ref<ModalType>({
  name: '',
  date: '',
  gender: '',
  country: '',
  email: '',
});

// Reset value
const reset = () => {
  loading.value = true;
  users.value = [];
};

// Fetch API
const getRandomUser = async () => {
  reset();
  // fetch data
  await fetch('https://randomuser.me/api?results=20', {
    method: 'GET',
    headers: {
      Accept: 'application/json',
    },
  }).then(async (data) => {
    const { results } = await data.json();
    users.value = results.map((result: any) => {
      return {
        datetime: result.registered.date,
        date: moment(result.registered.date).format('DD MMM YYYY'),
        name: `${result.name.first} ${result.name.last}`,
        gender: result.gender,
        country: result.location.country,
        email: result.email,
      };
    });
    loading.value = false;
  });
};

// Modal control
const showModal = (data: any) => {
  modalData.value = data;
  RefModal.value?.open();
};

// First fetch
onMounted(() => {
  getRandomUser();
});
</script>

<template>
  <main class="container">
    <!-- Datatable -->
    <DataTable
      :columns="columns"
      :items="users"
      :loading="loading"
      :clickable="true"
      @handleClick="showModal"
    />

    <!-- Modal -->
    <Modal ref="RefModal" :title="modalData.name">
      <div class="grid gap-4 grid-cols-6">
        <div class="capitalize text-gray-400">Date:</div>
        <div class="col-span-5">{{ modalData.date }}</div>
        <div class="capitalize text-gray-400">status:</div>
        <div class="col-span-5">Inactive</div>
        <div class="capitalize text-gray-400">gender:</div>
        <div class="col-span-5 capitalize">{{ modalData.gender }}</div>
        <div class="capitalize text-gray-400">country:</div>
        <div class="col-span-5">{{ modalData.country }}</div>
        <div class="capitalize text-gray-400">email:</div>
        <div class="col-span-5">{{ modalData.email }}</div>
      </div>
    </Modal>

    <!-- Refresh Button -->
    <div class="flex items-center justify-center">
      <button
        @click="getRandomUser"
        class="flex items-center justify-center text-white border-2 border-cyan-500 bg-cyan-500 w-25 py-2 px-4 rounded"
      >
        <mdicon name="refresh" size="20" class="mr-1 inline-block" />
        <p class="font-semibold">Refresh</p>
      </button>
    </div>
  </main>
</template>
