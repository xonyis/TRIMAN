<script >
import { RouterLink, RouterView, useRoute  } from 'vue-router'
import { ref } from 'vue';

export default {
  setup() {
    const showNavbar = ref(true);
    const handleChildEvent = (shouldShowNavbar) => {
      showNavbar.value = shouldShowNavbar;
    };

    return {
      showNavbar,
      handleChildEvent,
    };
  },
};
</script>

<template>
  <div class="main-container">
    <header>
      <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->

      <div class="wrapper" v-if="showNavbar">
        <nav>
          <!-- <RouterLink to="/">Home</RouterLink> -->
          <RouterLink to="/">Game</RouterLink>
          <RouterLink to="/about">About</RouterLink>
        </nav>
      </div>
    </header>
  <RouterView @updateNavbarVisibility="handleChildEvent"  />
  </div>
</template>

<style >


header {
  line-height: 1.5;
  max-height: 100vh;
  
}


nav {
  width: 100%;
  font-size: 16px;
  text-align: center;
  margin-top: .5rem;
  margin-bottom: .5rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
  color: var(--yellow);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  

  
}
</style>
