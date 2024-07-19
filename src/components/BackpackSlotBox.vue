<template>
  <div class="flex mx-4 my-4 shadow-xl shadow-red-800 rounded-xl p-4 w-full hover:bg-gray-800">
    <div class="flex flex-col font-bold text-xl">
      <div class="flex">
        <label for="item-name-1">Name:</label>
        <div class="flex justify-end w-full">
          <button class="text-red-700 font-bold" @click="$emit('delete')"><i class="fa-solid fa-trash"></i></button>
        </div>
      </div>
      <input
        :value="modelValue.name"
        @input="updateName"
        id="item-name-1"/>
    </div>
    <div class="flex flex-col font-bold text-xl w-full">
      <label for="item-name-1">Size:</label>
      <input
        type="number"
        :value="modelValue.size"
        @input="updateSize"
        id="item-name-1"/>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  modelValue: Object
});

let emit = defineEmits(['update:modelValue', 'delete']);

const updateName = (event) => {
  emit('update:modelValue', { ...props.modelValue, name: event.target.value });
};

const updateSize = (event) => {
  emit('update:modelValue', { ...props.modelValue, size: event.target.value });
};
</script>
