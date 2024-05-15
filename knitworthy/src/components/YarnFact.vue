<script setup lang="ts">
  import { computed } from 'vue'
  import lengths from "../lengths.json"
  import times from "../times.json"
  const unitString = computed(() => generateText());
  const timeString = computed(() => generateTimeText());

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
  const showEmptyError = defineModel("showEmptyError", {
    type: Boolean,
    required: true,
  })
  const showUnitString = defineModel("showUnitString", {
    type: Boolean,
    required: true,
  })
  const showTimeString = defineModel("showTimeString", {
    type: Boolean,
    required: true,
  })
  const showDefaultMessage = computed(() => !showUnitString.value && !showTimeString.value && !showEmptyError.value)
function generateText() {
  let coefficient = 1000000000000000;
  let index = 0;

    for (let i = 0; i < lengths.length; i++) {
      if ((lengths[i].length / unit.value) < coefficient && (lengths[i].length / unit.value) > 1) {
        coefficient = lengths[i].length / unit.value
        index = i;
      }
    }
  
    let factDesc = lengths[index].description ? `, ${lengths[index].description}, ` : " ";
    return `Your ${name.value} used ${unit.value} ${unitText.value.toLowerCase()} of yarn, which could span the ${lengths[index].dimension} of ${lengths[index].name}${factDesc}${coefficient.toFixed(2)} times`
}

function generateTimeText(): string {
  let exactMatch: string | null = null;
  times.forEach((entry) => {
    if (entry.time === time.value) {
      exactMatch = `It took ${time.value} hours to make ${name.value}. In that amount of time, you could ${entry.task}.`
    }
  })
  if (exactMatch) return exactMatch

  let multipleMatch: string | null = null;
  let i = 0;
  while (multipleMatch === null && i < times.length) {
    if ((times[i].time /time.value) > 1) {
      multipleMatch = `In the time it took you to make your ${name.value} project, you could ${times[i].task} ${(times[i].time / time.value).toFixed(2)} times.`
    }
    i++;
  }
  if (multipleMatch) return multipleMatch

  return 'No match found';
}
</script>

<template>
  <div class="flex-col">
    <p v-if="showDefaultMessage">Use the calculator to get a fact!</p>
    <p v-if="showEmptyError">Enter {{ unitText }} and/or Hours to get a fact!</p>
    <p v-if="showUnitString">{{ unitString }}</p>
    <p v-if="showTimeString">{{ timeString }}</p>
  </div>
</template>
