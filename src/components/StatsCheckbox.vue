<template>
<div class="w-full flex flex-row justify-between text-2xl">
  {{ props.attributeName }}:
  <span
    v-for="(checked, index) in props.checkboxes[attributeName]"
    :key="index"
    :id="`${props.attributeName}-stat-${index}`"
    :class="`${checked === true ? 'text-red-600' : 'text-white' }`"
    @click="$emit('setAttribute', checked, index, props.attributeName)"
  ><i class="fa-solid fa-skull"></i></span>
</div>
</template>
<script setup>
import { defineProps, ref, defineEmits } from 'vue'

const props = defineProps({
  attributeName: String,
  checkboxes: Array
})
const emits = defineEmits(['setAttribute'])
</script>
