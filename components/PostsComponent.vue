<template>
  <div class="p-5">
    <h1 class="text-3xl text-center">Посты</h1>
    <div v-if="paginatedPosts.length" class="grid grid-cols-3 grid-rows-auto gap-10 m-10">
      <div v-for="post in paginatedPosts" :key="post.id" class="border px-2 py-10">
        <span>{{ post.title }}</span>
        <p>{{ post.body }}</p>
      </div>
      
    </div>
    <div class="flex justify-center gap-2">
        <span
            @click="currentPage = page"
            :class="['cursor-pointer border p-2 rounded-full', currentPage === page ? 'bg-blue-600 text-white' : 'bg-gray-200 text-blue-600']"
            v-for="page in pageLength" :key="page"
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
const currentPage = ref(1)

const postsLength = ref(0); //Количество постов
const postsOnePage = 10; //Количество постов на одной странице

const pageLength = computed(() => Math.ceil(postsLength.value / postsOnePage)); //Количество страниц

// Вычисляем посты для текущей страницы
const paginatedPosts = computed(() => {
  const start = (currentPage.value - 1) * postsOnePage; //Начало постов
  const end = start + postsOnePage; //Конец постов
  return posts.value.slice(start, end); //Возвращаем новый обьект с постами
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
    getPosts()
})

</script>

<style>
</style>