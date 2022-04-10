<script setup>
import Task from './Task';
import {computed, defineEmit, defineProps} from "vue";

const emit = defineEmit(['archive-task', 'pin-task']);

const props = defineProps({
  tasks: { type: Array, required: true, default: () => [] },
  loading: { type: Boolean, default: false },
});

const isEmpty = computed(() => props.tasks.length === 0);

const tasksInOrder = computed(() => {
  return [
    ...props.tasks.filter(t => t.state ==='TASK_PINNED'),
    ...props.tasks.filter(t => t.state !=='TASK_PINNED'),
  ];
});

function onArchiveTask(taskId) {
  emit('archive-task', taskId);
}

function onPinTask(taskId) {
  emit('pin-task', taskId);
}

</script>

<template>
  <div class="list-items">
    <template v-if="loading">
      <div v-for="n in 6" :key="n" class="loading-item">
        <span class="glow-checkbox" />
        <span class="glow-text"> <span>Loading</span> <span>cool</span> <span>state</span> </span>
      </div>
    </template>
    <template v-else-if="isEmpty">
      <div class="wrapper-message">
        <span class="icon-check" />
        <div class="title-message">You have no tasks</div>
        <div class="subtitle-message">Sit back and relax</div>
      </div>
    </template>
    <template v-else>
      <Task
          v-for="task in tasksInOrder"
          :key="task.id"
          :task="task"
          @archive-task="onArchiveTask"
          @pin-task="onPinTask"
      />
    </template>
  </div>
</template>