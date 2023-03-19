<script setup lang="ts">
import type { Column, Task } from '~/types'
import draggable from 'vuedraggable'
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

const isAltKeyDown = useKeyModifier('Alt')
</script>

<template>
  <div>
    <draggable
      v-model="columns"
      group="columns"
      :animation="150"
      handle=".drag-handle"
      item-key="id"
      class="flex gap-4 overflow-x-auto items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            {{ column.title }}
          </header>

          <draggable
            v-model="column.tasks"
            :group="{ name: 'tasks', pull: isAltKeyDown ? 'clone' : true }"
            :animation="150"
            handle=".drag-handle"
            item-key="id"
          >
            <template #item="{ element: task }: { element: Task }">
              <div><Task :task="task" class="task" /></div>
            </template>
          </draggable>

          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>

<style>
/* Class added to an element when it's being dragged. */
.sortable-drag .task {
  transform: rotate(5deg);
}

/* Class added to the element underneath the element 
* being dragged to style the spot it can be dropped it in.
*/
.sortable-ghost .task {
  position: relative;
}
.sortable-ghost .task::after {
  content: '';
  @apply absolute top-0 bottom-0 right-0 left-0 bg-slate-300 rounded;
}
</style>
