<script setup>
import StarRating from '@/components/StarRating.vue'

defineProps({
  id: String,
  title: String,
  help: String
})

const rating = defineModel('rating')
const notes = defineModel('notes')
</script>

<template>
  <div class="p-2 w-full">
    <div class="flex flex-col gap-y-3">
      <div class="flex justify-between">
        <label :for="id" class="grow text-2xl text-gray-900 dark:text-white">
          {{ title }}
        </label>

        <button
          v-if="rating !== null"
          @click="rating = null"
          class="flex mx-auto text-black bg-yellow-500 border-0 py-2 px-8 focus:outline-none hover:bg-yellow-600 rounded"
        >
          Hide
        </button>

        <button
          v-else
          @click="rating = 3"
          class="flex mx-auto text-black bg-yellow-500 border-0 py-2 px-8 focus:outline-none hover:bg-yellow-600 rounded"
        >
          Show
        </button>
      </div>

      <StarRating v-show="rating !== null" v-model="rating" />

      <p v-if="rating !== null && help">
        {{ help }}
      </p>

      <textarea
        v-show="rating !== null"
        v-model="notes"
        :id="id"
        rows="4"
        class="w-full bg-gray-100 dark:bg-gray-800 bg-opacity-50 rounded border border-gray-300 dark:border-gray-700 focus:border-yellow-500 focus:bg-white dark:focus:bg-gray-900 focus:ring-2 focus:ring-yellow-200 dark:focus:ring-yellow-900 text-base outline-none text-gray-700 dark:text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
      ></textarea>
    </div>
  </div>
</template>

<style scoped></style>
