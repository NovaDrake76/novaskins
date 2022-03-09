<script>
import Home from "./components/Home.vue";
import Market from "./components/Market.vue";

const routes = {
  "/": Home,
  "/market": Market,
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
