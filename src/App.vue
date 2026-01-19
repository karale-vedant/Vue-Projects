<script setup>
import Welcome from "./components/pages/Welcome.vue";
import Layout from "./components/layouts/Layout.vue";
import Dashboard from "./components/pages/Dashboard.vue";
import Workout from "./components/pages/Workout.vue";

import { ref, computed,onMounted } from "vue";
import { workoutProgram } from "./utils";
const defaultData = {};
for (let workoutIdx in workoutProgram) {
  const workoutData = workoutProgram[workoutIdx];
  // Create a for loop where we iterate over every workout
  defaultData[workoutIdx] = {}; // initialize the workout data obj

  //nested loop to loop over every exercise within a workout, and initialize its input value to an empty string
  for (let e of workoutData.workout) {
    defaultData[workoutIdx][e.name] = "";
  }
}
console.log(defaultData);

const selectedDisplay = ref(1);
const data = ref(defaultData); //{1....30:{exercise_name:'',.....}}
const selectedWorkout = ref(-1);

const isWorkoutComplete = computed(() => {
  const currWorkout = data.value?.[selectedWorkout.value];
  if (!currWorkout) {
    return false;
  } // guard close to exit function

  const isCompleteCheck = Object.values(currWorkout).every((ex) => !!ex);
  console.log("ISCOMPLETE: ", isCompleteCheck);
  return isCompleteCheck;
});

const firstIncompleteWorkIndex = computed(() => {
  const allWorrkouts = data.value;
  if (!allWorrkouts) {
    return -1;
  }

  //loop every key vale pair and check is the workout is completed or not
  for (const [index, workout] of Object.entries(allWorrkouts)) {
    const isComplete = Object.values(workout).every((ex) => !!ex);
    if (!isComplete) {
      return parseInt(index);
    }
  }
  return -1; // all are complte
});

function handleChangeDisplay(idx) {
  selectedDisplay.value = idx;
}

function handleSelectWorkout(idx) {
  selectedDisplay.value = 3;
  selectedWorkout.value = idx;
}

function handleSaveWorkout(idx) {
  //Save the current data snapshot to localstorage so that we can retrivve it next time
  localStorage.setItem("workouts", JSON.stringify(data.value));
  //show the dashboard
  selectedDisplay.value = 2;
  //deselect a workout
  selectedWorkout.value = -1;
}

function handleResetPlan() {
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
  data.value = defaultData;
  localStorage = removeItem("workouts");
}

onMounted(() => {
  if(!localStorage) {return}
  if(localStorage.getItem('workouts')) {
    // only enter the if block we find some data saved to the key workouts in localstorage database
    const saveData = JSON.parse(localStorage.getItem('workouts'))
    data.value = saveData
    selectedDisplay.value = 2
  }
})

</script>

<template>
  <Layout>
    <Welcome
      :handleChangeDisplay="handleChangeDisplay"
      v-if="selectedDisplay == 1"
    />

    <Dashboard
      :handleResetPlan="handleResetPlan"
      :firstIncompleteWorkIndex="firstIncompleteWorkIndex"
      :handleSelectWorkout="handleSelectWorkout"
      v-if="selectedDisplay == 2"
    />

    <Workout
      :handleSaveWorkout="handleSaveWorkout"
      :isWorkoutComplete="isWorkoutComplete"
      :data="data"
      :selectedWorkout="selectedWorkout"
      v-if="workoutProgram?.[selectedWorkout]"
    />
  </Layout>
</template>

<style scoped></style>
