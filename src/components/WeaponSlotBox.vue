<template>
  <div class="flex mx-4 my-4 shadow-xl shadow-red-800 rounded-xl p-4 w-full hover:bg-gray-800">
    <div class="flex flex-row py-4 w-full">
      <div class="flex flex-col w-1/2">
        <label
          :for="`weapon-name-${index}`"
          class="text-xl">Name</label>
        <input
          :id="`weapon-name-${index}`"
          :value="modelValue.name"
          @input="updateName" />

        <label 
          :for="`weapon-skill-${index}`"
          class="text-xl">Skill</label>
        <input
          :id="`weapon-skill-${index}`"
          :value="modelValue.skill"
          @input="updateSkill" />

        <label 
          :for="`weapon-damage-${index}`"
           class="text-xl">Damage</label>
        <input 
          :id="`weapon-damage-${index}`"
          :value="modelValue.damage"
          @input="updateDamage" />

        <label :for="`weapon-range-${index}`" class="text-xl">Range</label>
        <input
          :id="`weapon-range-${index}`"
          :value="modelValue.range"
          @input="updateRange" />
      </div>
      <div class="flex flex-col w-1/2">
        <label :for="`weapon-ammo-${index}`" class="text-xl">Ammo</label>
        <input type="number"
          :id="`weapon-ammo-${index}`"
          :value="modelValue.ammo"
          @input="updateAmmo" />

        <label :for="`weapon-${index}-broken`" class="flex text-xl w-full">Broken</label>
          <i 
            class="fa-regular fa-circle text-2xl text-red-600 cursor-pointer"
            @click="updateBroken()" 
            :class="modelValue.broken ? `hidden` : ``"></i>
          <i 
            class="fas fa-circle-check text-2xl text-red-600 cursor-pointer"
            @click="updateBroken()" 
            :class="modelValue.broken ? `` : `hidden`"></i>

        <label :for="`weapon-size-${index}`" class="text-xl">Size</label>
        <input type="number"
          :value="modelValue.size"
          @input="updateSize"
          :id="`weapon-size-${index}`" />

        <label :for="`weapon-traits-${index}`" class="text-xl">Traits</label>
        <textarea
          :id="`weapon-traits-${index}`"
          :value="modelValue.traits"
          @input="updateTraits"/>
      </div>
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

const updateSkill = (event) => {
  emit('update:modelValue', { ...props.modelValue, skill: event.target.value });
};

const updateDamage = (event) => {
  emit('update:modelValue', { ...props.modelValue, damage: event.target.value });
};

const updateRange = (event) => {
  emit('update:modelValue', { ...props.modelValue, range: event.target.value });
};

const updateAmmo = (event) => {
  emit('update:modelValue', { ...props.modelValue, ammo: event.target.value });
};

const updateBroken = (event) => {
  if (props.modelValue.broken === true) {
    emit('update:modelValue', { ...props.modelValue, broken: false });
  } else {
    emit('update:modelValue', { ...props.modelValue, broken: true });
  }
};

const updateSize = (event) => {
  emit('update:modelValue', { ...props.modelValue, size: event.target.value });
};

const updateTraits = (event) => {
  emit('update:modelValue', { ...props.modelValue, traits: event.target.value });
};
</script>
