<script>
import Home from "./pages/Home.vue";
import Market from "./pages/Market.vue";
import Drop from "./pages/Drop.vue";
import Tracker from "./pages/Tracker.vue";

const routes = {
  "/": Home,
  "/market": Market,
  "/drop": Drop,
  "/tracker": Tracker,
};

export default {
  data() {
    return {
      currentPath: window.location.hash,
    };
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || "/"];
    },
  },
  mounted() {
    window.addEventListener("hashchange", () => {
      this.currentPath = window.location.hash;
    });
  },
};
</script>

<template>
  <component :is="currentView" />
</template>
<style>
@import url("assets/base.css");
</style>
