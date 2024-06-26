<template>
  <div class="flex flex-col text-red-600 px-4 py-4 w-full">
    <h1 class="text-italic font-bold text-center text-2xl"> Don't Stress And Survive </h1>
    <div class="flex flex-col text-2xl w-full">
      <div class="flex flex-row justify-between">
        <button class="text-center border-2 border-red-600 bg-transparent border-4 p-8 rounded-xl" @click="downloadData">Export</button>
      </div>
      <div class="flex flex-row justify-between">
        <button class="text-center border-2 border-red-600 bg-transparent border-4 p-8 rounded-xl" @click="importData">Import</button>
      </div>
    </div>
    <div class="flex flex-row text-2xl border-b-2 border-red-600 rounded-xl p-8 w-full">
      <label for="name">Name:</label>
      <input id="name" v-model="name" class="text-2xl bg-transparent rounded-XL px-4 focus:border-red-700" />
    </div>
    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Statistics</p>
    </div>
    <div class="flex flex-row py-4 w-full">
      <div class="flex flex-col px-2 w-full" id="statistics-column-a">
        <label for="max-hp" class="text-xl"><i class="fa-solid fa-heart text-2xl"></i> Max Health:</label>
        <input id="max-hp" v-model="statMaxHp" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"  disabled/>
        <label for="current-hp" class="text-xl">Current Health:</label>
        <input id="current-hp" v-model="currentHp" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700" />
        <label for="max-mp" class="text-xl"><i class="fa-solid fa-wand-sparkles text-2xl"></i> Max Magic:</label>
        <input id="max-mp" v-model="statMaxMp" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700" />
        <label for="current-mp">Current Magic:</label>
        <input id="current-mp" v-model="currentMp" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700" />
      </div>
      <div class="flex flex-col px-2 w-full" id="statistics-column-b">
        <label for="max-hp" class="text-xl"> <i class="fa-solid fa-ghost text-2xl"></i> Current Stress:</label>
        <input type="number" id="max-hp" v-model="stress" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700" />
        <label for="current-hp" class="text-xl"> <i class="fa-solid fa-clover text-2xl"></i>Current Fortune:</label>
        <input id="current-hp" v-model="fortune" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700" />
      </div>
    </div>
    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Throw Helper</p>
    </div>
    <div class="flex flex-row py-4 px-8 w-full rounded-xl border-b-2 border-red-600">
      <span v-if="isSkillSelected" class="text-2xl">
        Current Dice Pull: {{ currentDicePull }} (Remember to check talents!)
      </span>
    </div>
    <div class="flex flex-row py-4 w-full">
      <div class="flex flex-col px-2 w-full" id="attributes-column">
        <p class="text-2xl w-full text-center">Attributes</p>
        <StatsCheckbox
          v-for="(value, key) in attributes"
          :key="key"
          :attributeName="key"
          :checkboxes="attributesCheckboxes"
          :marked="markedAttributes[key]"
          @setAttribute="setAttribute"
          @setMarked="setMarkedAttribute"
          class="px-2 py-2 my-2"
          />
      </div>
      <div class="flex flex-col px-2 w-full">
        <p class="text-2xl w-full text-center">Skills</p>
        <StatsCheckbox
          v-for="(value, key) in coulmnASkills"
          :key="key"
          :attributeName="key"
          :checkboxes="skillsCheckboxes"
          :marked="markedSkills[key]"
          @setAttribute="setSkill"
          @setMarked="setMartedSkill"
          class="px-2 py-2 my-2"
          />
      </div>
      <div class="flex flex-col px-2 w-full">
        <p class="text-2xl w-full text-center"></p>
        <StatsCheckbox
          v-for="(value, key) in coulmnBSkills"
          :key="key"
          :attributeName="key"
          :checkboxes="skillsCheckboxes"
          :marked="markedSkills[key]"
          @setAttribute="setSkill"
          @setMarked="setMartedSkill"
          class="px-2 py-2 my-2"
          />
      </div>
    </div>
    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Wounds</p>
    </div>
    <div class="w-full rounded-xl border-b-2 border-red-600">
      <StatsCheckbox
        v-for="(value, key) in wounds"
        :key="key"
        :attributeName="key"
        :checkboxes="woundsCheckboxes"
        @setAttribute="setWound"
        class="flex flex-row px-2 py-2 my-2"
        />
    </div>
    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Inventory</p>
    </div>
    <div
      class="flex flex-row py-4 w-full"
      v-for="(item, i) in inventorySlots" :key="i">
      <InventoryBox v-model="inventorySlots[i]"/>
    </div>
    <div class="flex flex-row py-4 w-full border-b-2 border-red-600 rounded-xl">
    </div>
  </div>
</template>
<script setup>
import InventoryBox from './InventoryBox.vue';
import StatsCheckbox from './StatsCheckbox.vue';
import { ref, watchEffect, watch, computed } from 'vue'
const Attributes = [
  'Strength', 'Agility', 'Intelligence', 'Wits', 'Willpower', 'Power', 'Charisma'
];
const Skills = [
  'Atletics', 'Brawl', 'Intimidation', 'Throwing', 'LongArms', 'ShortArms', 'Dodge', 'Driving', 'Stealth',
  'Observation', 'Listening', 'Sense', 'Inquire', 'Survival', 'Medicine', 'Science', 'Tech', 'Nature', 'Electronics', 'SlieghtOfHand',
  'Toughness', 'Mind', 'Hiding', 'Persuasion', 'Speech', 'Lying', 'SmallTalk', 'Animals'
];
const strenthRelatedSkills = [
  'Atletics', 'Brawl', 'Intimidation', 'Throwing'
];
const agilityRelatedSkills = [
  'LongArms', 'ShortArms', 'Dodge', 'Driving', 'Stealth'
]; 
const intelligenceRelatedSkills = [
  'Observation', 'Listening', 'Sense', 'Inquire', 'Survival'
];
const witsRelatedSkills = [
  'Medicine', 'Science', 'Tech', 'Nature', 'Electronics', 'SlieghtOfHand'
];
const willpowerRelatedSkills = [
  'Toughness', 'Mind', 'Hiding'
];
const charismaRelatedSkills = [
  'Persuasion', 'Speech', 'Lying', 'SmallTalk', 'Animals'
];

const hitLocations = [
  'Head', 'Torso', 'Left Arm', 'Right Arm', 'Left Leg', 'Right Leg'
];

const InventorySlot = {
  name: '',
  description: ''
};

const saRelations = {
  Strength: strenthRelatedSkills,
  Agility: agilityRelatedSkills,
  Intelligence: intelligenceRelatedSkills,
  Wits: witsRelatedSkills,
  Willpower: willpowerRelatedSkills,
  Charisma: charismaRelatedSkills,
};
const statMax = 4;
const skillMax = 5;
const woundsMax = 4;
const attributes = ref(arrToObj(Attributes, 0));
const attributesCheckboxes = ref(arrToObj(Attributes, Array(statMax).fill(false)));

const skills = ref(arrToObj(Skills, 0));

const skillsCheckboxes = ref(arrToObj(Skills, Array(skillMax).fill(false)));

const markedAttributes = ref(arrToObj(Attributes, false));

const markedSkills = ref(arrToObj(Skills, false));

const name = ref('Character Name');

const stress = ref(0);
const fortune = ref(0);
const currentHp = ref(0);
const currentMp = ref(0);

const wounds = ref(arrToObj(hitLocations, 0));

const woundsCheckboxes = ref(arrToObj(hitLocations, Array(woundsMax).fill(false)));

const inventorySlots = ref(Array(5).fill(InventorySlot));

const isSkillSelected = computed(() => {
  return Object.values(markedSkills.value).some(v => v === true);
});

const currentMarkedSkill = computed(() => {
  return Object.keys(markedSkills.value).find(k => markedSkills.value[k] === true);
});

const currentDicePull = computed(() => {
  if (isSkillSelected.value === false) {
    return 0;
  }
  let attributeDice = "";
  let skillDice = "";
  let attributeValue = 0;
  let skillValue = skills.value[currentMarkedSkill.value];
  for (const [k, v] of Object.entries(saRelations)) {
    if (v.includes(currentMarkedSkill.value)) {
      attributeValue = attributes.value[k];
    }
  }

  return `${getAttributeDice(attributeValue)} + ${getSkillDice(skillValue)} Stres: ${getStressDice()}`;
});

const coulmnASkills = computed(() => {
  return Object.fromEntries(Object.entries(skills.value).slice(0, 12));
});

const coulmnBSkills = computed(() => {
  return Object.fromEntries(Object.entries(skills.value).slice(13, 27));
});

const statMaxHp = computed(() => {
  return attributes.value['Strength'] + attributes.value['Willpower'] + 3;
});

const statMaxMp = computed(() => {
  return attributes.value['Power'] + attributes.value['Willpower'];
});

watch(attributes, (newVal) => {
  for (const [key, value] of Object.entries(newVal)) {
    let v = Array(statMax).fill(false);
    for (let i = 0; i < value; i++) {
      v[i] = true;
    }
    attributesCheckboxes.value[key] = v;
  }
}, { deep: true });

watch(skills, (newVal) => {
  for (const [key, value] of Object.entries(newVal)) {
    let v = Array(skillMax).fill(false);
    for (let i = 0; i < value; i++) {
      v[i] = true;
    }
    skillsCheckboxes.value[key] = v;
  }
}, { deep: true })

watch(wounds, (newVal) => {
  for (const [key, value] of Object.entries(newVal)) {
    let v = Array(woundsMax).fill(false);
    for (let i = 0; i < value; i++) {
      v[i] = true;
    }
    woundsCheckboxes.value[key] = v;
  }
}, { deep: true });

function setAttribute(checked, value, index) {
  let val = checked ? value : value + 1;
  attributes.value[index] = val;
}
function setSkill(checked, value, index) {
  let val = checked ? value : value + 1;
  skills.value[index] = val;
}
function setMarkedAttribute(value) {
  if (saRelations[value].length === null) {
    return;
  }
  markedAttributes.value[value] = !markedAttributes.value[value];
  for (const [k, v] of Object.entries(markedAttributes.value)) {
    if (value !== k) {
      markedAttributes.value[k] = false;
    }
  }

  for (const [k, v] of Object.entries(markedSkills.value)) {
    if (saRelations[value].includes(k)) {
      markedSkills.value[k] = markedAttributes.value[value];
    } else {
      markedSkills.value[k] = false;
    }
  }
}

function setMartedSkill(value) {
  markedSkills.value[value] = !markedSkills.value[value];
  for (const [k, v] of Object.entries(markedSkills.value)) {
    if (value !== k) {
      markedSkills.value[k] = false;
    }
  }
  for (const [k, v] of Object.entries(markedAttributes.value)) {
    markedAttributes.value[k] = false;
  }
}

function setWound(checked, value, index) {
  let val = checked ? value : value + 1;
  wounds.value[index] = val;
}

function arrToObj(arr, defaultValue = false) {
  return Object.fromEntries(arr.map((v, i) => [v, defaultValue]));
}
function getAttributeDice(attributeValue) {
  switch (attributeValue) {
    case 0:
      return "0";
    case 1:
      return "1d6";
    case 2:
      return "1d8";
    case 3:
      return "1d10";
    case 4:
      return "1d12";
  }
}
function getSkillDice(skillValue) {
  switch (skillValue) {
    case 0:
      return "0";
    case 1:
      return "1d4";
    case 2:
      return "1d6";
    case 3:
      return "1d8";
    case 4:
      return "1d10";
    case 5:
      return "1d12";
  }
}
function getStressDice() {
  switch (stress.value) {
    case 0:
      return "0";
    case 1:
      return "1d6";
    case 2:
      return "1d6";
    case 3:
      return "1d6";
    case 4:
      return "1d8";
    case 5:
      return "2d6";
    case 6:
      return "2d6";
    case 7:
      return "2d6";
    case 8:
      return "2d6";
    case 9:
      return "2d6";
    case 10:
      return "3d4";
  }
}

function downloadData() {
  const data = {
    name: name.value,
    statMaxHp: statMaxHp.value,
    currentHp: currentHp.value,
    statMaxMp: statMaxMp.value,
    currentMp: currentMp.value,
    stress: stress.value,
    fortune: fortune.value,
    attributes: attributes.value,
    skills: skills.value,
    wounds: wounds.value,
    inventorySlots: inventorySlots.value
  };
  const json = JSON.stringify(data);
  const blob = new Blob([json], { type: 'application/json' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = 'data.json';
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};

function importData() {
  const input = document.createElement('input');
  input.type = 'file';
  input.accept = '.json';
  input.onchange = (event) => {
    const file = event.target.files[0];
    const reader = new FileReader();
    reader.onload = (event) => {
      const data = JSON.parse(event.target.result);
      name.value = data.name;
      currentHp.value = data.currentHp;
      currentMp.value = data.currentMp;
      stress.value = data.stress;
      fortune.value = data.fortune;
      attributes.value = data.attributes;
      skills.value = data.skills;
      wounds.value = data.wounds;
      inventorySlots.value = data.inventorySlots;
      // ... update all your other ref variables ...
    };
    reader.readAsText(file);
  };
  input.click();
};
</script>
