<template>
  <div class="flex flex-col mx-4 my-4 shadow-xl shadow-red-800 rounded-xl p-4 w-full hover:bg-gray-800 w-full">
    <div class="flex p-2 w-full">
      <p class="text-center text-xl font-bold w-full"> {{ name }} </p>
    </div>
    <div class="flex flex-col py-4 w-full">
        <div class="w-full py-2">
          <label :for="`clothing-${name}-name`" class="text-xl w-full px-2">Name</label>
          <input :id="`clothing-${name}-name`"
            :value="modelValue.name"
            @input="updateName"/>
        </div>
        <div class="w-full py-2">
          <label :for="`clothing-${name}-armor`" class="text-xl w-full px-2">Armor</label>
          <input :id="`clothing-${name}-armor`"
            :value="modelValue.armor"
            @input="updateArmor"/>
        </div>

        <div class="w-full py-2">
          <label :for="`clothing-${name}-traits`" class="flex text-xl w-full px-2">Traits</label>
          <textarea :id="`clothing-${name}-traits`"
            :value="modelValue.traits"
            class="w-full"
            @input="updateTraits"/>
        </div>

        <div class="w-full py-2">
          <label :for="`clothing-${name}-broken`" class="flex text-xl w-full px-2">Broken</label>
            <i 
              class="fa-regular fa-circle text-2xl text-red-600 cursor-pointer"
              @click="updateBroken()" 
              :class="modelValue.broken ? `hidden` : ``"></i>
            <i 
              class="fas fa-circle-check text-2xl text-red-600 cursor-pointer"
              @click="updateBroken()" 
              :class="modelValue.broken ? `` : `hidden`"></i>
        </div>
    </div>
  </div>
</template>
<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  modelValue: Object,
  name: String
});

let emit = defineEmits(['update:modelValue', 'delete']);

const updateName = (event) => {
  emit('update:modelValue', { ...props.modelValue, name: event.target.value });
};

const updateBroken = (event) => {
  if (props.modelValue.broken === true) {
    emit('update:modelValue', { ...props.modelValue, broken: false });
  } else {
    emit('update:modelValue', { ...props.modelValue, broken: true });
  }
};

const updateArmor = (event) => {
  emit('update:modelValue', { ...props.modelValue, armor: event.target.value });
};

const updateTraits = (event) => {
  emit('update:modelValue', { ...props.modelValue, traits: event.target.value });
};
</script>
