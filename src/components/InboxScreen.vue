<script setup>
import { useStore } from 'vuex';
import PureInboxScreen from './PureInboxScreen';
import {computed} from "vue";

const store = useStore();
const error = computed(() => store.state.error);

</script>

<template>
  <PureInboxScreen :error="error" />
</template>