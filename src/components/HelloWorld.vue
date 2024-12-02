<script setup>
defineProps({
  msg: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="greetings">
    <h1>{{ msg }}</h1>
    <h3>
      Bienvenue tu veux faire une partie ?
    </h3>
  </div>
</template>

<style scoped>

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  color: var(--yellow);
  margin: auto;
  margin-top: 1rem;
}

h3 {
  font-size: 1rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}


</style>
