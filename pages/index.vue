<script setup>
import { useBoardStore } from '../stores/boardStore'

const boardStore = useBoardStore()
const route = useRoute()
const router = useRouter()

const newColumnName = ref('')

const isModalOpen = computed(() => {
  return route.name === 'index-tasks-id'
})

function addColumn() {
  boardStore.addColumn(newColumnName.value)
  newColumnName.value = ''
}

const nbrOfTasks = computed(() => boardStore.board.columns.reduce((t, c) => t + c.tasks.length, 0))

function closeModal() {
  router.push('/')
}
</script>

<template>
  <div class="board-wrapper">
    <h1 class="mb-2">Backlog: {{ nbrOfTasks }} task(s)</h1>
    <main class="board">
      <BoardColumn
        v-for="(column, columnIndex) in boardStore.board.columns"
        :key="column.id"
        :column="column"
        :columnIndex="columnIndex"
      />
      <UContainer class="column">
        <UInput
          v-model="newColumnName"
          type="text"
          placeholder="Create new column"
          icon="i-heroicons-plus-circle-solid"
          @keyup.enter="addColumn"
        />
      </UContainer>
    </main>
    <div v-show="isModalOpen" class="task-bg" @click.self="closeModal">
      <NuxtPage :key="route.fullPath" />
    </div>
  </div>
</template>
