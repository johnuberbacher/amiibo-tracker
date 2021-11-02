<template>
  <div id="app">
    <AmiiboTracker msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import AmiiboTracker from "./components/AmiiboTracker.vue";

export default {
  name: "App",
  components: {
    AmiiboTracker,
  },
};
</script>

<style>
* {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.card,
.card:hover {
  text-decoration: none;
}
.card {
  transition: 0.2s background;
}
.card:hover {
  background: var(--light);
}
.shadow {
  transition:  0.2s box-shadow;
}
.shadow:hover {
  box-shadow: 0 0.15rem 0.25rem rgba(0, 0, 0, 0.05) !important;
}
.card.collected {
  -webkit-filter: grayscale(100%);
  filter: grayscale(100%);
}
</style>
