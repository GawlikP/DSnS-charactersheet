<template>
<div
  class="w-full flex flex-row justify-between text-2xl"
  :class="props.marked ? 'shadow-md shadow-rose-600' : ''">
  <span @click="$emit('setMarked', attributeName)"> {{ props.attributeName }}: </span>
  <span
    v-for="(checked, index) in props.checkboxes[attributeName]"
    :key="index"
    :id="`${props.attributeName}-stat-${index}`"
    :class="`${checked === true ? 'text-red-600' : 'text-stone-500' } hover:text-red-500 hover:cursor-pointer`"
    @click="$emit('setAttribute', checked, index, props.attributeName)"
  ><i class="fa-solid fa-skull"></i></span>
</div>
</template>
<script setup>
import { defineProps, ref, defineEmits } from 'vue'

const props = defineProps({
  attributeName: String,
  checkboxes: Object,
  marked: Boolean
})
const emits = defineEmits(['setAttribute', 'setMarked'])
</script>
