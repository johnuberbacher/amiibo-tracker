<template>
  <div id="app">
    <AmiiboTracker />
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
  transition: 0.2s box-shadow;
}
.shadow:hover,
.card.collected {
  box-shadow: 0 0.15rem 0.25rem rgba(0, 0, 0, 0.05) !important;
}
.card.collected {
  background: #dedede;
  -webkit-filter: grayscale(100%);
  filter: grayscale(100%);
}
.ribbon {
  font-size: 0.9rem;
  letter-spacing: 0.5px;
  right: 0;
  top: 2rem;
  --ribbon-size: 1rem;
  padding-inline-start: calc(var(--ribbon-size));
  border-radius: 0.25rem;
  z-index: 3;
  color: #fff;
  width: fit-content;
  clip-path: polygon(0 0, var(--ribbon-size) 50%, 0 100%, 100% 100%, 100% 0);
}
.dropdown-menu {
  max-height: 200px;
  overflow-y: auto;
}

::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

/* Track */
::-webkit-scrollbar-track {
  border-radius: 100vh;
  background: var(--light);
}

/* Handle */
::-webkit-scrollbar-thumb {
  --tw-bg-opacity: 1;
  background-color: var(--primary);
  border-radius: 100vh;
  border: none;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background-color: rgba(16, 185, 129, var(--tw-bg-opacity));
  border: none;
}
</style>
