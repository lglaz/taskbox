<script setup>
import {computed, defineProps, defineEmit} from "vue";

const emit = defineEmit(['archive-task', 'pin-task'])

const props = defineProps({task: {
    type: Object,
    required: true,
    default: () => ({id: '', state: '', title: ''}),
    validator: task => ['id', 'state', 'title'].every(key => key in task),
  }
});

const classes = computed(() => ({
  'list-item TASK_INBOX': props.task.state === 'TASK_INBOX',
  'list-item TASK_PINNED': props.task.state === 'TASK_PINNED',
  'list-item TASK_ARCHIVED': props.task.state === 'TASK_ARCHIVED',
}));

const isChecked = computed(() => props.task.state === 'TASK_ARCHIVED');

function archiveTask() {
  emit('archive-task', props.task.id);
}

function pinTask() {
  emit('pin-task', props.task.id);
}

</script>

<template>
  <div :class="classes">
    <label class="checkbox">
      <input type="checkbox" :checked="isChecked" disabled name="checked" />
      <span class="checkbox-custom" @click="archiveTask()" :aria-label="'archiveTask-' + task.id" />
    </label>
    <div class="title">
      <input type="text" :value="task.title" readonly placeholder="Input title" style="background: red;" />
    </div>
    <div class="actions">
      <a v-if="!isChecked" @click="pinTask()">
        <span class="icon-star" :aria-label="'pinTask-' + task.id" />
      </a>
    </div>
  </div>
</template>