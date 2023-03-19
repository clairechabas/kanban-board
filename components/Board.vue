<script setup lang="ts">
import type { Column, Task } from '~/types'
import draggable from 'vuedraggable'
import { nanoid } from 'nanoid'

let columns = ref<Column[]>([
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

function blurElement(event: KeyboardEvent) {
  const element = event.target as HTMLInputElement
  element.blur()
}

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: [],
  }

  // Add new column in columns array
  columns.value.push(column)

  // Wait for Vue to have finished updating the DOM
  nextTick(() => {
    // Focus the new column's title input
    const newColumnInputTitle = document.querySelector(
      '.column:last-of-type .title-input-column'
    ) as HTMLInputElement

    newColumnInputTitle.focus()
  })
}

function deleteColumn(column: Column) {
  column.title === ''
    ? (columns.value = columns.value.filter((col) => col.id !== column.id))
    : null
}
</script>

<template>
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable
      v-model="columns"
      group="columns"
      :animation="150"
      handle=".drag-handle"
      item-key="id"
      class="flex gap-4 items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            <input
              class="title-input-column bg-transparent focus:bg-white rounded px-1 w-4/5"
              type="text"
              @keyup.enter="blurElement($event)"
              @keydown.backspace="deleteColumn(column)"
              v-model="column.title"
            />
          </header>

          <draggable
            v-model="column.tasks"
            :group="{ name: 'tasks', pull: isAltKeyDown ? 'clone' : true }"
            :animation="150"
            handle=".drag-handle"
            item-key="id"
          >
            <template #item="{ element: task }: { element: Task }">
              <div>
                <Task
                  :task="task"
                  @delete="
                    column.tasks = column.tasks.filter(
                      (task) => task.id !== $event
                    )
                  "
                />
              </div>
            </template>
          </draggable>

          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>

    <button
      @click="createColumn"
      class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
    >
      + Add Another Column
    </button>
  </div>
</template>
