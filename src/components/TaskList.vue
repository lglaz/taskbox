<script setup>
import PureTaskList from './PureTaskList';
import {useStore} from "vuex";
import {computed} from "vue";

const store = useStore();

const tasks  = computed(() => store.state.tasks);
const archiveTask = task => store.dispatch('archiveTask', task);
const pinTask = task => store.dispatch('pinTask', task);

</script>

<template>
  <PureTaskList :tasks="tasks" @archive-task="archiveTask" @pin-task="pinTask" />
</template>