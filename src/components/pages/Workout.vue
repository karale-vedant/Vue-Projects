<template>
  <Portal hello="world" :handleCloseModal="handleCloseModal" v-if="selectedExercise">
    <div class="exercise-description">
      <h3>{{ selectedExercise }}</h3>
      <div>
        <small>Description</small>
        <p>{{ exerciseDescription }}</p>
      </div>
      <button @click="handleCloseModal">
        Close <i class="fa-solid fa-xmark"></i>
      </button>
    </div>
  </Portal>
  <section id="workout-card card">
    <div class="plan-card card">
      <div class="plan-card-header">
        <p>
          Day
          {{ selectedWorkout < 9 ? "0" + (selectedWorkout + 1) : selectedWorkout }}
        </p>
        <i class="fa-solid fa-dumbbell"></i>
      </div>
      <h2>{{ workoutType[selectedWorkout % 3] }} Workout</h2>
    </div>
    <div class="workout-grid">
      <h4 class="grid-name">Warmup</h4>
      <h6>Sets</h6>
      <h6>Reps</h6>
      <h6 class="grid-weights">Weights</h6>
      <div class="workout-grid-row" v-for="(w, wIdx) in warmup" :key="wIdx">
        <div class="grid-name">
          <p>{{ w.name }}</p>
          <button
            @click="
              () => {
                // console.log('Clicked the button' + w.name);
                selectedExercise = w.name;
              }
            "
          >
            <i class="fa-regular fa-circle-question"></i>
          </button>
        </div>
        <p>{{ w.sets }}</p>
        <p>{{ w.reps }}</p>
        <input class="grid-weight" placeholder="14kg" type="text" disabled />
      </div>
      <div class="workout-grid-line"></div>
      <h4 class="grid-name">Workout</h4>
      <h6>Sets</h6>
      <h6>Reps</h6>
      <h6 class="grid-weights">Weights</h6>
      <div class="workout-grid-row" v-for="(w, wIdx) in workout" :key="wIdx">
        <div class="grid-name">
          <p>{{ w.name }}</p>
          <button
            @click="
              () => {
                selectedExercise = w.name;
              }
            "
          >
            <i class="fa-regular fa-circle-question"></i>
          </button>
        </div>
        <p>{{ w.sets }}</p>
        <p>{{ w.reps }}</p>
        <input v-model="data[selectedWorkout][w.name]" class="grid-weight" placeholder="14kg" type="text" />
      </div>
    </div>
    <div class="card workout-btns">
      <button @click="handleSaveWorkout">Save & Exit <i class="fa-solid fa-save"></i></button>
      <button :disabled="!isWorkoutComplete" @click="handleSaveWorkout">Complete <i class="fa-solid fa-check"></i></button>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";
import Portal from "../Portal.vue";
import { exerciseDescriptions, workoutProgram } from "@/utils";

const workoutType = ['Push', 'Pull', 'Legs']

const {data, selectedWorkout} = defineProps({
  data: Object,
  selectedWorkout: Number,
  handleSaveWorkout: Function,
  isWorkoutComplete: Boolean
})


const { workout, warmup } = workoutProgram[selectedWorkout];
//const selectedExercise = null;
let selectedExercise = ref(null);
console.log(selectedExercise);
const exerciseDescription = computed(
  () => exerciseDescriptions[selectedExercise.value]
);

function handleCloseModal() {
  selectedExercise.value = null;
}
</script>

<style scoped>
#workout-card,
.plan-card {
  display: flex;
  flex-direction: column;
}

#workout-card {
  gap: 1.5rem;
}

.plan-card-header,
.workout-btns {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}

.workout-grid,
.workout-grid-row {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 1rem;
}


.workout-grid {
  padding-top: 2rem;
  padding-bottom: 1rem;
}

.workout-grid-row,
.workout-grid-line {
  grid-column: span 7 / span 7;
}

.workout-grid-line {
  margin: 0.5rem 0;
  height: 3px;
  border-radius: 2px;
  background: var(--background-muted);
}

.grid-name {
  grid-column: span 3 / span 3;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.grid-name button {
  padding: 0;
  border: none;
  box-shadow: none;
}

.grid-name button:hover {
  transform: none;
  box-shadow: none;
  color: var(--color-link);
}

.workout-grid-row .grid-name button {
  opacity: 0;
  pointer-events: none;
}

.workout-grid-row:hover .grid-name button {
  opacity: 1;
  pointer-events: all;
}

.grid-name p {
  text-transform: capitalize;
}

.grid-weight {
  grid-column: span 2 / span 2;
}

.workout-btns button {
  flex: 1;
}

.exercise-description {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 100%;
}

.exercise-description h3 {
  text-transform: capitalize;
}

.exercise-description button {
  padding-left: 0.5rem;
}
</style>