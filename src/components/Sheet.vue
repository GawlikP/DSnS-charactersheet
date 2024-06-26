<template>
  <div class="flex flex-col text-red-600 px-4 py-4 w-full">
    <h1 class="text-italic font-bold text-center text-2xl"> Don't Stress And Survive </h1>
    <div class="flex flex-col text-md w-full">
      <div class="flex">
        <button class="text-center border-2 border-red-600 bg-transparent border-2 p-2 rounded-xl hover:shadow-md hover:shadow-rose-500" @click="downloadData">Export</button>
        <button class="text-center border-2 border-red-600 bg-transparent border-2 p-2 rounded-xl hover:shadow-md hover:shadow-rose-500" @click="importData">Import</button>
      </div>
    </div>
    <div class="flex flex-row text-2xl border-b-2 border-red-600 rounded-xl p-8 w-full">
      <label for="name">Name:</label>
      <input id="name" v-model="name" />
    </div>
    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Statistics</p>
    </div>
    <div class="flex flex-row py-4 w-full">
      <div class="flex flex-col px-2 w-full" id="statistics-column-a">
        <label for="max-hp" class="text-xl"><i class="fa-solid fa-heart text-2xl"></i> Max Health:</label>
        <input id="max-hp" v-model="statMaxHp"  disabled/>
        <label for="current-hp" class="text-xl">Current Health:</label>
        <input id="current-hp" v-model="currentHp" />
        <label for="max-mp" class="text-xl"><i class="fa-solid fa-wand-sparkles text-2xl"></i> Max Magic:</label>
        <input id="max-mp" v-model="statMaxMp" />
        <label for="current-mp">Current Magic:</label>
        <input id="current-mp" v-model="currentMp"/>
      </div>
      <div class="flex flex-col px-2 w-full" id="statistics-column-b">
        <label for="fear" class="text-xl"> <i class="fa-solid fa-ghost text-2xl"></i> Current Fear:</label>
        <input type="number" id="fear" v-model="fear"/>
        <label for="fortune" class="text-xl"> <i class="fa-solid fa-clover text-2xl"></i>Current Fortune:</label>
        <input id="fortune" v-model="fortune" />
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
      <p class="text-2xl w-full text-center">Equipment</p>
    </div>
    <div class="flex flex-row py-4 w-full">
      <div class="flex flex-col w-1/2">
        <label for="equipment-hands" class="text-xl">Hands</label>
        <input id="equipment-hands" v-model="equipment.Hands" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>

        <label for="equipment-pants-pockets" class="text-xl">Pants Pockets</label>
        <input id="equipment-pants-pockets" v-model="equipment.PantsPockets" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>

        <label for="equipment-jacket-pockets" class="text-xl">Jacket Pockets</label>
        <input id="equipment-jacket-pockets" v-model="equipment.JacketPockets" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>

        <label for="equipment-small-holster-one" class="text-xl">Small Holster One</label>
        <input id="equipment-small-holster-one" v-model="equipment.SmallHolsterOne" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>
      </div>
      <div class="flex flex-col w-1/2">
        <label for="equipment-small-holster-two" class="text-xl">Small Holster Two</label>
        <input id="equipment-small-holster-two" v-model="equipment.SmallHolsterTwo" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>

        <label for="equipment-large-holster" class="text-xl">Large Holster</label>
        <input id="equipment-large-holster" v-model="equipment.LargeHolster" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>

        <label for="equipment-scabbard" class="text-xl">Scabbard</label>
        <input id="equipment-scabbard" v-model="equipment.Scabbard" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>

        <label for="equipment-little-things" class="text-xl">Little Things</label>
        <input id="equipment-little-things" v-model="equipment.LittleThings" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>
      </div>
    </div>
    <div class="flex flex-row py-4 w-full border-b-2 border-red-600 rounded-xl"></div>

    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Talents</p>
    </div>

    <div class="flex flex-row py-4 w-full">
      <button class="text-center border-2 border-red-600 bg-transparent p-2 rounded-xl" @click="addTalentSlot">AddTallent</button>
    </div>
    <div
      class="flex flex-row py-4 w-full"
      v-for="(talent, i) in talentSlots" :key="i">
      <TalentBox v-model="talentSlots[i]" @delete="deleteTalentSlot(i)" :index="i"/>
    </div>
    <div class="flex flex-row py-4 w-full border-b-2 border-red-600 rounded-xl"></div>


    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Inventory</p>
    </div>

    <div class="flex flex-row py-4 w-full">
        <label for="backpack-capacity" class="text-xl">Backpack/Bag capacity:</label>
        <input id="backpack-capacity" v-model="backpack.capacity" class="text-2xl bg-transparent rounded-xl px-4 focus:border-red-700"/>
    </div>
    <div class="flex flex-row py-4 w-full">
      <p class="text-md w-full text-center">Capacity: {{ backpack.capacity }} / {{ usedBackpackSlots }}</p>
    </div>
    <div class="flex flex-row py-4 w-full">
      <button class="text-center border-2 border-red-600 bg-transparent p-2 rounded-xl" @click="addBackpackSlot">Add Item</button>
    </div>
    <div
      class="flex flex-row py-4 w-full"
      v-for="(slot, i) in backpack.slots" :key="i">
      <BackpackSlotBox v-model="backpack.slots[i]" @delete="deleteBackpackSlot(i)"/>
    </div>
    <div class="flex flex-row py-4 w-full border-b-2 border-red-600 rounded-xl"></div>

    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Weapons</p>
    </div>

    <div class="flex flex-row py-4 w-full">
      <button class="text-center border-2 border-red-600 bg-transparent p-2 rounded-xl" @click="addWeapon">Add Weapon</button>
    </div>

    <div class="flex flex-row py-4 w-full"
      v-for="(slot, i) in weapons" :key="i">
      <WeaponSlotBox v-model="weapons[i]" @delete="deleteWeapon(i)" :index="i"/>
    </div>
    <div class="flex flex-row py-4 w-full border-b-2 border-red-600 rounded-xl"></div>


    <div class="flex flex-row py-4 w-full">
      <p class="text-2xl w-full text-center">Clothes</p>
    </div>

    <div class="flex flex-row py-4 w-full border-b-2 border-red-600 rounded-xl"></div>

    <div class="flex flex-row py-4 w-full"
      v-for="(slot, i) in clothing" :key="i">
      <ClothSlotBox v-model="clothing[i]" :name="i"/>
    </div>
  </div>
</template>
<script setup>
import ClothSlotBox from './ClothSlotBox.vue';
import WeaponSlotBox from './WeaponSlotBox.vue';
import StatsCheckbox from './StatsCheckbox.vue';
import TalentBox from './TalentBox.vue';
import BackpackSlotBox from './BackpackSlotBox.vue';
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

const TalentSlot = {
  name: '',
  description: '',
  darkSide: ''
};

const BackpackSlot = {
  name: '',
  size: 1
}

const WeaponSlot = {
  name: '',
  skill: '',
  damage: '',
  range: '',
  ammo: '',
  broken: false,
  traits: '',
  size: 0,
}

const ClothSlot = {
  name: '',
  armor: 0,
  traits: '',
  broken: false,
}
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

const fear = ref(0);
const fortune = ref(0);
const currentHp = ref(0);
const currentMp = ref(0);

const wounds = ref(arrToObj(hitLocations, 0));

const woundsCheckboxes = ref(arrToObj(hitLocations, Array(woundsMax).fill(false)));

const inventorySlots = ref(Array(5).fill(InventorySlot));

const talentSlots = ref(Array(1).fill(TalentSlot));

const clothing = ref(arrToObj(hitLocations, ClothSlot));

const backpack = ref({
  capacity: 10,
  slots: Array(1).fill(BackpackSlot)
});

const isSkillSelected = computed(() => {
  return Object.values(markedSkills.value).some(v => v === true);
});

const currentMarkedSkill = computed(() => {
  return Object.keys(markedSkills.value).find(k => markedSkills.value[k] === true);
});

const equipment = ref({
  Hands: '',
  PantsPockets: '',
  JacketPockets: '',
  SmallHolsterOne: '',
  SmallHolsterTwo: '',
  LargeHolster: '',
  Scabbard: '',
  LittleThings: ''
});

const weapons = ref(Array(1).fill(WeaponSlot))

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

  return `${getAttributeDice(attributeValue)} + ${getSkillDice(skillValue)} Fear: ${getFearDice()}`;
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

const usedBackpackSlots = computed(() => {
  let initial = 0;
  return backpack.value.slots.reduce((acc, slot) => parseInt(acc) + parseInt(slot.size), initial);
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

function addInventoryBox() {
  inventorySlots.value.push(InventorySlot);
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
function getFearDice() {
  switch (fear.value) {
    case 0:
      return "0";
    case 1:
      return "1d12";
    case 2:
      return "1d12";
    case 3:
      return "1d10";
    case 4:
      return "1d8";
    case 5:
      return "2d8";
    case 6:
      return "2d6";
    case 7:
      return "2d6";
    case 8:
      return "2d6";
    case 9:
      return "2d4";
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
    fear: fear.value,
    fortune: fortune.value,
    attributes: attributes.value,
    skills: skills.value,
    wounds: wounds.value,
    inventorySlots: inventorySlots.value,
    equipment: equipment.value,
    backpack: backpack.value,
    talentSlots: talentSlots.value,
    weapons: weapons.value,
    clothing: clothing.value
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
      fear.value = data.fear;
      fortune.value = data.fortune;
      attributes.value = data.attributes;
      skills.value = data.skills;
      wounds.value = data.wounds;
      inventorySlots.value = data.inventorySlots;
      equipment.value = data.equipment;
      backpack.value = data.backpack;
      talentSlots.value = data.talentSlots;
      weapons.value = data.weapons;
      clothing.value = data.clothing;
    };
    reader.readAsText(file);
  };
  input.click();
};

function deleteItemSlot(index) {
  inventorySlots.value.splice(index, 1);
};
function deleteTalentSlot(index) {
  talentSlots.value.splice(index, 1);
};
function addTalentSlot() {
  talentSlots.value.push(TalentSlot);
};
function addBackpackSlot() {
  if (usedBackpackSlots.value >= backpack.value.capacity) {
    return;
  }
  backpack.value.slots.push(BackpackSlot);
};
function deleteBackpackSlot(index) {
  backpack.value.slots.splice(index, 1);
};
function addWeapon() {
  weapons.value.push(WeaponSlot);
};
</script>
<style>
input {
 @apply text-lg bg-transparent rounded-xl px-4 focus:border-red-700 focus:bg-stone-800 focus:outline-none focus:ring-2 focus:ring-red-700;
}
textarea {
 @apply text-lg bg-transparent rounded-xl px-4 focus:border-red-700 focus:bg-stone-800 focus:outline-none focus:ring-2 focus:ring-red-700;
}
</style>
