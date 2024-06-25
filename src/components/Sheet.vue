<template>
  <div class="flex flex-col text-red-600 px-4 py-4">
    <div class="flex flex-row">
      <p class="text-2xl"> Name: {{ name }} </p>
    </div>
    <div class="flex flex-row">
      <div class="flex flex-col px-2">
        <StatsCheckbox
          v-for="(value, key) in attributes"
          :key="key"
          :attributeName="key"
          :checkboxes="attributesCheckboxes"
          @setAttribute="setAttribute"
          />
      </div>
      <div class="flex flex-col px-2">
      </div>
    </div>
  </div>
</template>
<script setup>
import StatsCheckbox from './StatsCheckbox.vue';
import { ref, watchEffect, watch } from 'vue'
const statMax = 5;
const attributes = ref({
  Strength: 0,
  Agility: 0,
  Intelligence: 0,
  Wits: 0,
  Willpower: 0,
  Power: 0,
  Charisma: 0,
});
const attributesCheckboxes = ref({
  Strength: Array(statMax).fill(false),
  Agility: Array(statMax).fill(false),
  Intelligence: Array(statMax).fill(false),
  Wits: Array(statMax).fill(false),
  Willpower: Array(statMax).fill(false),
  Power: Array(statMax).fill(false),
  Charisma: Array(statMax).fill(false),
});
const name = ref('Character Name');

const strengthCheckboxes = ref(Array(statMax).fill(false));

watch(strengthCheckboxes, (newVal) => {
}, { deep: true });

watch(attributes, (newVal) => {
  for (const [key, value] of Object.entries(newVal)) {
    let v = Array(statMax).fill(false);
    for (let i = 0; i < value; i++) {
      v[i] = true;
    }
    attributesCheckboxes.value[key] = v;
  }
}, { deep: true });
function setAttribute(checked, value, index) {
  let val = checked ? value : value + 1;
  attributes.value[index] = val;
}
</script>
