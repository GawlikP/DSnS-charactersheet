<template>
  <div class="flex flex-col mx-4 my-4 shadow-xl shadow-red-800 rounded-xl w-full hover:bg-gray-800 p-4">
    <div class="flex font-bold text-xl w-full">
      <div class="w-max-full">
        <label :for="`talent-name-${index}`">Name:</label>
        <div class="flex justify-end w-full">
          <button class="text-red-700 font-bold" @click="$emit('delete')"><i class="fa-solid fa-trash"></i></button>
        </div>
      </div>
      <input
        class="w-full"
        :id="`talent-name-${index}`"
        :value="modelValue.name"
        @input="updateName"/>
    </div>
    <div class="flex flex-col font-bold text-xl w-full">
      <label :for="`talent-description-${index}`">Description:</label>
      <textarea
        :id="`talent-description-${index}`"
        :value="modelValue.description"
        @input="updateDescription" />
    </div>
    <div class="flex flex-col font-bold text-xl w-full">
      <label :for="`talent-darkSide-${index}`">Dark Side:</label>
      <textarea
        :id="`talent-darkSide-${index}`"
        :value="modelValue.darkSide"
        @input="updateDarkSide" />
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  modelValue: Object,
  index: Number
});

let emit = defineEmits(['update:modelValue', 'delete']);

const updateName = (event) => {
  emit('update:modelValue', { ...props.modelValue, name: event.target.value });
};

const updateDescription = (event) => {
  emit('update:modelValue', { ...props.modelValue, description: event.target.value });
};
const updateDarkSide = (event) => {
  emit('update:modelValue', { ...props.modelValue, darkSide: event.target.value });
};
</script>
