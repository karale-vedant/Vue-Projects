<script setup>
defineProps({
  handleChangeDisplay: Function
})
</script>

<template>
  <section id="welcome">
    <div class="benefits">
      <h2>Complete this Program<br />if you want.....</h2>
    </div>
    <div>
      <h3>✅Follow a simple program with proven results.</h3>
      <h3>✅Get fit, healthy, strong and absolutely shreaded.</h3>
      <h3>✅Learn more about gym and technique</h3>
    </div>
    <div>
      <h3>The Training Plan</h3>
      <p>
        This Training plan follows a structure known as the<strong
          >Bro Split</strong
        >.
      </p>
      <h5><i>Push &rarr; Pull &rarr; Legs &rarr; Repeat</i></h5>
    </div>
    <div id="card" class="card">
      <h3>🔥Your Challenge</h3>
      <p>
        Complete all the workouts and track your progresss along the way!<br />Do
        you accept?
      </p>
      <button @click="handleChangeDisplay(2)">Begin &rarr;</button>
    </div>
  </section>
</template>

<style scoped>
#welcome,
.challenge,
.benefits {
  display: flex;
  flex-direction: column;
}

#welcome {
  gap: 1.5rem;
}

.benefits {
  gap: 0.5rem;
}

.challenge {
  gap: 0.35rem;
}

button:hover {
  color: rgb(0, 0, 0);
  background-color: rgb(193, 185, 185);
}

#card:hover {
  background-color: rgb(90, 90, 197);
  color: black;
}

@media (min-width: 640px) {
    #welcome {
        gap: 2rem;
        padding: 1rem 0;
    }
    .benefits {
      gap: 0.5rem;
    }
}
</style>