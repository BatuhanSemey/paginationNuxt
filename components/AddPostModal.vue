<template>
  <div>
    <button
      class="px-4 py-2 bg-blue-500 text-white rounded-lg"
      @click="showModal = !showModal"
    >
      Добавить новый пост
    </button>

    <div
      v-if="showModal"
      class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-50"
    >
      <div class="bg-white p-6 rounded-lg shadow-lg max-w-md w-full">
        <h2 class="text-2xl font-semibold mb-4">Добавить новый пост</h2>
        <form @submit.prevent="addPost">
          <div class="mb-4">
            <label for="title" class="block text-sm font-medium text-gray-700"
              >Заголовок</label
            >
            <input
              v-model="newPost.title"
              type="text"
              id="title"
              required
              class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500"
            />
          </div>
          <div class="mb-4">
            <label for="body" class="block text-sm font-medium text-gray-700"
              >Текст</label
            >
            <textarea
              id="body"
              v-model="newPost.body"
              required
              class="mt-1 block w-full border-gray-300 rounded-md shadow-sm focus:border-indigo-500 focus:ring-indigo-500"
            ></textarea>
          </div>
          <div class="flex justify-end">
            <button
              @click="showModal = !showModal"
              type="button"
              class="px-4 py-2 mr-2 bg-gray-500 text-white rounded-lg"
            >
              Отмена
            </button>
            <button
              type="submit"
              class="px-4 py-2 bg-blue-500 text-white rounded-lg"
            >
              Добавить
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const showModal = ref(false);
const newPost = ref({
  title: "",
  body: "",
});

const addPost = async () => {
  showModal.value = !showModal.value;

  try {
    const response = await axios.post(
      "https://my-json-server.typicode.com/BatuhanSemey/myJSONServer/posts",
      newPost.value
    );
    console.log(response);
  } catch (error) {
    console.log(error);
  }
};
</script>

<style>
</style>