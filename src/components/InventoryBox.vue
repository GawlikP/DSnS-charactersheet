<template>
  <div class="mx-4 my-4 shadow-xl shadow-red-800 rounded-xl p-4 w-full hover:bg-gray-800">
    <div class="flex flex-col w-1/4 font-bold text-xl">
      <label for="item-name-1">Name:</label>
      <input
        :value="modelValue.name"
        @input="updateName"
        id="item-name-1"
        class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700" />
    </div>
    <div class="flex flex-col w-full font-bold text-xl">
      <label for="item-name-1">Description:</label>
      <textarea
        :value="modelValue.description"
        @input="updateDescription"
        id="item-name-1"
        class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700 w-full resize-y"/>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  modelValue: Object
});

let emit = defineEmits(['update:modelValue']);

const updateName = (event) => {
  emit('update:modelValue', { ...props.modelValue, name: event.target.value });
};

const updateDescription = (event) => {
  emit('update:modelValue', { ...props.modelValue, description: event.target.value });
};
</script>
