<script setup lang="ts">
import type { Column } from '~/types'
import { nanoid } from 'nanoid'

const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: 'Backlog',
    tasks: [
      {
        id: nanoid(),
        title: 'Create a Laravel backend for this Vue app',
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: 'Deploy this app',
        createdAt: new Date(),
      },
    ],
  },
  {
    id: nanoid(),
    title: 'Doing',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'Tested',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'Deployed',
    tasks: [],
  },
  {
    id: nanoid(),
    title: 'Done',
    tasks: [],
  },
])
</script>

<template>
  <div class="flex gap-4 overflow-x-auto items-start">
    <div
      v-for="column in columns"
      :key="column.id"
      class="bg-gray-200 p-5 rounded min-w-[250px]"
    >
      <header class="font-bold mb-4">{{ column.title }}</header>
      <Task v-for="task in column.tasks" :task="task" :key="task.id" />

      <footer>
        <button class="text-gray-500">+ Add a Card</button>
      </footer>
    </div>
  </div>
</template>
