<template>
  <div>
    <the-header></the-header>
    <the-chooser
      v-if="mode === null"
      @set-mode="setMode"
      @set-settings="setSettings"
    ></the-chooser>
    <the-problem
      v-if="mode"
      :mode="mode"
      :multipliers="multipliers"
      :rows="rows"
    ></the-problem>
  </div>
</template>

<script>
import TheHeader from "./components/TheHeader.vue";
import TheChooser from "./components/TheChooser.vue";
import TheProblem from "./components/TheProblem.vue";
import { computed } from "vue";

export default {
  name: "App",
  components: {
    TheHeader,
    TheChooser,
    TheProblem,
  },
  data() {
    return {
      mode: null,
      multipliers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      rows: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      rowPicks: {
        n12: { nr: 12, checked: true },
        n11: { nr: 11, checked: true },
        n10: { nr: 10, checked: true },
        n9: { nr: 9, checked: true },
        n8: { nr: 8, checked: true },
        n7: { nr: 7, checked: true },
        n6: { nr: 6, checked: true },
        n5: { nr: 5, checked: true },
        n4: { nr: 4, checked: true },
        n3: { nr: 3, checked: true },
        n2: { nr: 2, checked: true },
        n1: { nr: 1, checked: true },
      },
      timeLimit: 10
    };
  },
  provide() {
    return {
      multipliers: computed(() => this.multipliers),
      rows: computed(() => this.rows),
      rowPicks: this.rowPicks,
      toSolve: parseInt(process.env.VUE_APP_TOSOLVE),
      timeLimit: computed(() => this.timeLimit),
    };
  },
  methods: {
    setMode(mode) {
      this.mode = mode;
    },
    setSettings(multipliers, rows, rowPicks) {
      this.multipliers = multipliers;
      this.rows = rows;
      this.rowPicks = rowPicks;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Acme&display=swap");
* {
  font-family: "Acme", sans-serif;
}

a {
  text-decoration: none;
}
</style>
