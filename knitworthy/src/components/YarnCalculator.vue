<script setup lang="ts">
  import { ref } from 'vue'
  import lengths from "../lengths.json"
  import times from "../times.json"
  const yardage = ref(0);
  const time = ref(0);
  const name = ref("");
  const yardageString = ref("");
  const timeString = ref("");
  let showYardageString = ref(false);
  let showTimeString = ref(false);
  function clickHandler() {
    if (yardage.value > 0) {
      yardageString.value = generateText(name.value, yardage.value)
    }

    if (time.value > 0) {
      timeString.value = generateTimeText(name.value, time.value)
    }

    if (yardageString.value.length > 0) showYardageString.value = true;
    if (timeString.value.length > 0) showTimeString.value = true;
  }
  function generateText(name: string, size: number) {
  let coefficient = 1000000000000000;
  let index = 0;

    for (let i = 0; i < lengths.length; i++) {
      if ((lengths[i].length / size) < coefficient && (lengths[i].length / size) > 1) {
        coefficient = lengths[i].length / size 
        index = i;
      }
    }
  
    let factDesc = lengths[index].description
    return `The yarn in your ${name} project could span the ${lengths[index].dimension} of ${lengths[index].name}, ${factDesc}, ${coefficient.toFixed(2)} times`
}

function generateTimeText(name: string, time: number): string {
  let exactMatch: string | null = null;
  times.forEach((entry) => {
    if (entry.time === time) {
      exactMatch = `In the time it took you to make your ${name} project, you could ${entry.task}.`
    }
  })
  if (exactMatch) return exactMatch

  let multipleMatch: string | null = null;
  let i = 0;
  while (multipleMatch === null && i < times.length) {
    if ((times[i].time /time) > 1) {
      multipleMatch = `In the time it took you to make your ${name} project, you could ${times[i].task} ${(times[i].time / time).toFixed(2)} times.`
    }
    i++;
  }
  if (multipleMatch) return multipleMatch

  return 'No match found';
}
</script>

// button to switch between yards, meters, and grams

<template>
  <h1>Knitworthy Calculator</h1>
  <div>
    <h2>Calculator</h2>
    <label for="name">Project Name</label>
    <input id="name" type="text" v-model="name" required />
    <label for="yardage">Yardage</label>
    <input id="yardage" type="number" v-model="yardage" />
    <label for="yardage">Hours</label>
    <input id="time" type="number" v-model="time" />
    <button @click="clickHandler">Confirm</button>
  </div>
  <div>
    <h2>Knitworthy Fact</h2>
    <p v-if="showYardageString">{{ yardageString }}</p>
    <p v-if="showTimeString">{{ timeString }}</p>
  </div>
</template>