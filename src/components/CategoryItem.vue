<script setup lang="ts">
import { defineProps } from 'vue'
import router from '@/router'
import type { Data } from '@/models/Data'

const props = defineProps<{
  title: string,
  description: string,
  id: number,
  revision?: boolean,
}>()

function openCategory(): void {
  router.push({ name: 'Themes', params: { categoryId: props.id } })
}

function deleteCategory(): void {
  let dataFromStorage = localStorage.getItem('data')
  if (dataFromStorage) {
    let data: Data = JSON.parse(dataFromStorage).data
    data.categories = data.categories.filter(category => category.id !== props.id)
    localStorage.setItem('data', JSON.stringify({ data }))
    router.go(0)
  }
}

</script>

<template>
  <div @click="openCategory()">
    <h2>{{ props.title }}</h2>
    <p>{{ props.description }}</p>
    <img v-if="!revision" @click.stop="deleteCategory()" src="../assets/trash-can-regular.svg" alt="Delete">
  </div>
</template>

<style scoped>
div {
  width: 80vw;
  margin: 0.25em auto;
  padding: 1em 2em;
  border-radius: 5px;
  background-color: white;
}

img {
  right: 1em;
  height: 1em;
}

@media (prefers-color-scheme: dark) {
  div {
    background-color: var(--vt-c-black-mute);
  }
}

</style>