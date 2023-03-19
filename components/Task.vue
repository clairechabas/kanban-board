<script setup lang="ts">
import type { Task, ID } from '@/types'

const props = defineProps<{
  task: Task
}>()

const emit = defineEmits<{
  (e: 'delete', payload: ID): void
}>()

const focused = ref(false)
onKeyStroke('Backspace', (e) => {
  if (focused.value) emit('delete', props.task.id)
})

const createdDateAsLocaleString =
  props.task.createdAt instanceof Date
    ? props.task.createdAt.toLocaleDateString()
    : new Date(props.task.createdAt).toLocaleDateString()
</script>

<template>
  <div
    :title="createdDateAsLocaleString"
    class="task bg-white p-2 mb-2 rounded shadow-sm max-w-[250px] flex"
    @focus="focused = true"
    @blur="focused = false"
    tabindex="0"
  >
    <DragHandle class="pr-2" />
    <span>{{ task.title }}</span>
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

.task:focus,
.task:focus-visible {
  @apply outline-gray-400 !important;
  outline: gray auto 1px;
}
</style>
