<script setup lang="ts">
import { ref, computed } from 'vue'

const columns = [{
  key: 'id',
  label: 'ID'
}, {
  key: 'title',
  label: 'Title'
}, {
  key: 'description',
  label: 'Description'
}, {
  key: 'price',
  label: 'Price'
}, {
  key: 'rating',
  label: 'Rating'
},{
  key: 'brand',
  label: 'Brand'
}, {
  key: 'category',
  label: 'Category'
}, {
  key: 'thumbnail',
  label: 'Thumbnail'
}]

const people = ref([{
  id: 1,
  name: 'Lindsay Walton',
  title: 'Front-end Developer',
  email: 'lindsay.walton@example.com',
  role: 'Member'
}, {
  id: 2,
  name: 'Courtney Henry',
  title: 'Designer',
  email: 'courtney.henry@example.com',
  role: 'Admin'
}, {
  id: 3,
  name: 'Tom Cook',
  title: 'Director of Product',
  email: 'tom.cook@example.com',
  role: 'Member'
}, {
  id: 4,
  name: 'Whitney Francis',
  title: 'Copywriter',
  email: 'whitney.francis@example.com',
  role: 'Admin'
}, {
  id: 5,
  name: 'Leonard Krasner',
  title: 'Senior Designer',
  email: 'leonard.krasner@example.com',
  role: 'Owner'
}, {
  id: 6,
  name: 'Floyd Miles',
  title: 'Principal Designer',
  email: 'floyd.miles@example.com',
  role: 'Member'
}])


const { data } = await useFetch<any>('https://dummyjson.com/products');
let productslist = data.value.products;

const q = ref('');
const page = ref(1);
const pageCount = 5;
const pagemem = 1;

const filteredData = computed(() => {
  if (!q.value) {
    return productslist;
  }

  return productslist.filter((products) => {
    page.value = 1;
    return Object.values(products).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase());
    });
  });
});

const rows = computed(() => {
  return filteredData.value.slice((page.value - 1) * pageCount, page.value * pageCount);
});

</script>

<template>
  <div>
    <div class="flex px-3 py-3.5 border-b border-gray-200 dark:border-gray-700">
      <UInput v-model="q" placeholder="Filter people..." />
    </div>
    <UTable :rows="rows" :columns="columns">
      <template #thumbnail-data="{row}">
        <img :src="row.thumbnail" alt="" class="w-[100px] h-[100px]">
      </template>
      <template #description-data="{row}">
        <div class="text-wrap">{{ row.description }}</div>
      </template>
      <!-- Rating cell -->
      <template #rating-data="{row}">
        <div :class="{ 'bg-red-500/40': row.rating < 4.5, 'bg-green-500/40': row.rating >= 4.5 }" class="px-3 py-2">
          {{ row.rating }}
        </div>
      </template>

    </UTable>
    <div class="flex justify-center px-3 py-3.5 border-t border-gray-200 dark:border-gray-700">
      <UPagination v-model="page" :page-count="pageCount" :total="productslist.length" />
    </div>
  </div>
</template>