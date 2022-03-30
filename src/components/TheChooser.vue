<template>
  <the-settings
    v-if="showSettings"
    @close-settings="showSettings = false"
    @set-settings="setSettings"
  ></the-settings>
  <div>
    <button @click.prevent="setMode(1)">·</button>
    <button @click.prevent="setMode(2)">:</button>
    <button @click.prevent="setMode(3)">· & :</button>
  </div>
  <div>
    <button @click.prevent="showSettings = true">⚙️</button>
  </div>
</template>

<script>
import TheSettings from "./TheSettings.vue";

export default {
  emits: ["set-mode", "set-settings"],
  components: {
    TheSettings,
  },
  data() {
    return {
      showSettings: false,
      currentMultipliers: null,
      currentRows: null
    };
  },
  methods: {
    setMode(mode) {
      this.$emit("set-mode", mode);
    },
    setSettings(multipliers, rows, rowPicks) {
      console.log(multipliers)
      this.$emit("set-settings", multipliers, rows, rowPicks)
    }
  },
};
</script>

<style scoped>
div {
  margin-top: 4rem;
  text-align: center;
}

button {
  border: solid 1px lightgrey;
  background-color: white;
  margin: 0.5rem;
  padding: 0.5rem;
  width: 4rem;
  font-size: 2rem;
  border-radius: 20px;
  cursor: pointer;
}

button:hover {
  background-color: lightgrey;
}
</style>
