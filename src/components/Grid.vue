<template>
  <section id="grid">
    <button
      :disabled="workoutIdx > 0 && workoutIdx > firstIncompleteWorkIndex"
      @click="handleSelectWorkout(workoutIdx)"
      :key="workoutIdx"
      v-for="(workout, workoutIdx) in Object.keys(workoutProgram)"
      class="card-button plan-card"
    >
      <div>
        <p>
          Day {{ workoutIdx < 9 ? "0" + (workoutIdx + 1) : workoutIdx + 1 }}
        </p>
        <i class="fa-solid fa-dumbbell" v-if="workoutIdx % 3 == 0"></i>
        <i class="fa-solid fa-weight-hanging" v-if="workoutIdx % 3 == 1"></i>
        <i class="fa-solid fa-bolt" v-if="workoutIdx % 3 == 2"></i>
      </div>
      <h3>{{ workoutTypes[workoutIdx % 3] }}</h3>
    </button>
    <button :disabled="firstIncompleteWorkIndex != -1" @click="handleResetPlan" class="card-button plan-card-reset">
      <p>Reset</p>
      <i class="fa-solid fa-rotate-left"></i>
    </button>
  </section>
</template>

<script setup>
import { workoutProgram } from "@/utils";

defineProps({
  handleSelectWorkout: Function,
  firstIncompleteWorkIndex: Number,
  handleResetPlan: Function
});

const workoutTypes = ["Push", "Pull", "Legs"];
</script>

<style scoped>
#grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1rem;
}

#grid button {
  width: 100%;
}

#grid button:disabled {
  box-shadow: none;
}

.plan-card,
.plan-card-reset {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

.plan-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}

@media (min-width: 640px) {
  #grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }
}

h3 {
  color: rgb(2, 117, 219);
}
</style>
