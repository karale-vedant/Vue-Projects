<template>
  <section id="dashboard">
    <div class="card tip-container">
      <h2>Welcome to Pulse Fit</h2>
      <div>
        <p class="tip"><strong>Daily Tips</strong><br />{{ todaysFact }}</p>
      </div>
      <button @click="() => handleSelectWorkout(firstIncompleteWorkIndex < 0 ? 0 : firstIncompleteWorkIndex)">Start Workout &rarr;</button>
    </div>
    <Grid v-bind="props" />
  </section>
</template>

<script setup>
import { gymHealthFacts } from "../../utils";
import Grid from "../Grid.vue";

const props = defineProps({
  handleSelectWorkout: Function,
  firstIncompleteWorkIndex: Number
})

//Generate a random whole integer number between 1 and array length - 1
const randomNumber = Math.floor(Math.random() * gymHealthFacts.length);
const todaysFact = gymHealthFacts[randomNumber];
</script>

<style scoped>
.tip-container,
.tip-container div,
#dashboard {
  display: flex;
}

.tip-container,
#dashboard {
  flex-direction: column;
}

#dashboard {
  gap: 2rem;
}

.tip-container {
  gap: 0.5rem;
}

@media (min-width: 640px) {
  .tip-container {
    gap: 1rem;
  }
}
</style>
