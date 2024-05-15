<script setup lang="ts">
  const emit = defineEmits(["submit", "update:unit-text", "value-update"]);
  const unitText = defineModel("unitText", {
    type: String,
    required: true,
  })
  const unit = defineModel("unit", {
    type: Number,
    required: true,
  })
  const time = defineModel("time", {
    type: Number,
    required: true,
  })
  const name = defineModel("name", {
    type: String,
    required: true,
  })
  const unitTextOptions = ["Yards", "Meters", "Grams"];

  function handleChangeUnit() {
    const index = unitTextOptions.findIndex((option) => option === unitText.value);
    let newText = "Yards"
    if (index > -1 && index < 2) {
      newText = unitTextOptions[index + 1];
    }
    emit("update:unit-text", newText)
  }
</script>

<template>
  <form class="flex-col" @submit.prevent="$emit('submit')" >
    <label for="name">Project Name</label>
    <input id="name" type="text" v-model="name" @change.prevent="$emit('value-update')" required />
    <label for="unit">{{ unitText }}</label>
    <div class="flex-row unit-row">
      <input id="unit" type="number" min="0" v-model.number="unit" @input.prevent="$emit('value-update')" />
      <button class="unit-button" type="button" @click="handleChangeUnit">Switch Unit</button>
    </div>
    <label for="time">Hours</label>
    <input id="time" type="number" min="0" v-model.number="time" @input.prevent="$emit('value-update')" />
    <button class="confirm-button" type="submit" >Confirm</button>
  </form>
</template>

<style scoped>
  .confirm-button {
    margin: 1rem;
  }

  .flex-col > *, .unit-button {
    margin: .25rem;
  }
</style>
