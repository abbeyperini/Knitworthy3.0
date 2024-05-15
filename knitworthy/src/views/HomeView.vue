<script setup lang="ts">
  import YarnCalculator from '../components/YarnCalculator.vue'
  import YarnFact from "../components/YarnFact.vue";
  import { ref } from 'vue'
  const unitText = ref("Yards");
  const unit = ref(0);
  const time = ref(0);
  const name = ref("");
  let showUnitString = ref(false);
  let showTimeString = ref(false);
  let showEmptyError = ref(false);

  function handleUpdate() {
    showUnitString.value = false;
    showTimeString.value = false;
  }

  function handleSubmit() {
    showEmptyError.value = false;
    showUnitString.value = false;
    showTimeString.value = false;

    if ((!unit.value || unit.value === 0) && (!time.value || time.value === 0)) showEmptyError.value = true;

    if (unit.value > 0) showUnitString.value = true;
    if (time.value > 0) showTimeString.value = true;
  }
</script>

<template>
  <main>
    <div class="flex-col">
      <h1>Knitworthy Calculator</h1>
      <YarnCalculator
        v-model:unit-text="unitText"
        v-model:unit.number="unit"
        v-model:time.number="time"
        v-model:name="name"
        @submit="handleSubmit" 
        @value-update="handleUpdate"
        />
      <h2>Knitworthy Fact</h2>
      <YarnFact 
        v-model:unit-text="unitText"
        v-model:unit.number="unit"
        v-model:time.number="time"
        v-model:name="name"
        v-model:show-unit-string="showUnitString"
        v-model:show-time-string="showTimeString"
        v-model:show-empty-error="showEmptyError"
      />
    </div>
  </main>
</template>
