<template>
  <div class="p-5">
    <h1 class="text-3xl text-center">Посты</h1>

    <!-- Сортирока постов -->
    <div
      class="m-10 cursor-pointer hover:border-none active:border-none flex items-center gap-3 justify-between"
    >
      <AddPostModal />
      <select
        @change="sortNameChange($event)"
        class="border border-gray-300 rounded-lg p-2 bg-white text-gray-700 hover:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-500"
        name=""
        id=""
      >
        <option value="По возрастанию">По возрастанию</option>
        <option value="По убыванию">По убыванию</option>
      </select>
    </div>

    <!-- Посты -->
    <div
      v-if="paginatedPosts.length"
      class="grid grid-cols-3 grid-rows-auto gap-10 m-10"
    >
      <div
        v-for="post in paginatedPosts"
        :key="post.id"
        class="border px-2 py-10 rounded-lg p-2 bg-white text-gray-700 hover:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-500"
      >
        <span>{{ post.title }}</span>
        <p>{{ post.body }}</p>
      </div>
    </div>

    <!-- Пагинация постов -->
    <div class="flex justify-center gap-2">
      <span
        @click="currentPage = page"
        :class="[
          'cursor-pointer border p-2 rounded-full',
          currentPage === page
            ? 'bg-blue-600 text-white'
            : 'bg-gray-200 text-blue-600',
        ]"
        v-for="page in pageLength"
        :key="page"
      >
        {{ page }}
      </span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import axios from "axios";

const posts = ref([]);
const currentPage = ref(1);
const sortName = ref("По возрастанию");

const postsLength = ref(0); //Количество постов
const postsOnePage = 10; //Количество постов на одной странице

const pageLength = computed(() => Math.ceil(postsLength.value / postsOnePage)); //Количество страниц

// Сортировка постов
const sortNameChange = (event: any) => {
  sortName.value = event.target.value;
};

// Вычисляем посты для текущей страницы
const paginatedPosts = computed(() => {
  const start = (currentPage.value - 1) * postsOnePage; //Начало постов
  const end = start + postsOnePage; //Конец постов

  //Возвращаем новый обьект с постами + сортировка
  if (sortName.value === "По возрастанию") {
    return posts.value.slice(start, end).sort((a, b) => a.id - b.id);
  } else if (sortName.value === "По убыванию") {
    return posts.value.slice(start, end).sort((a, b) => b.id - a.id);
  }
});

const getPosts = async () => {
  try {
    const response = await axios.get(
      "https://my-json-server.typicode.com/BatuhanSemey/myJSONServer/posts"
    );
    posts.value = response.data;
    postsLength.value = posts.value.length;
    console.log(posts.value);
    console.log(`Количество постов: ${postsLength.value}`);
    console.log(`Количество страниц: ${pageLength.value}`);
  } catch (error) {
    console.error(error);
  }
};

onMounted(() => {
  getPosts();
});
</script>

<style>
</style>