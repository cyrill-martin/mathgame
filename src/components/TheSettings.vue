<template>
  <teleport to="body">
    <div class="backdrop"></div>
    <transition name="settings" appear>
      <div class="popUp">
        <div class="multipliers">
          <input
            type="radio"
            id="all12"
            :value="all12"
            v-model="currMultipliers"
          />

          <label for="all12">1-12</label>
          <br />
          <input
            type="radio"
            id="all10"
            :value="all10"
            v-model="currMultipliers"
          />
          <label for="all10">1-10</label>
        </div>
        <div>
          <input
            type="checkbox"
            id="n12"
            :value="currRowPicks.n12.nr"
            v-model="currRowPicks.n12.checked"
          />
          <label for="n12">x 12</label>
          <br />
          <input
            type="checkbox"
            id="n11"
            :value="currRowPicks.n11.nr"
            v-model="currRowPicks.n11.checked"
          />
          <label for="n11">x 11</label>
          <br />
          <input
            type="checkbox"
            id="10"
            :value="currRowPicks.n10.nr"
            v-model="currRowPicks.n10.checked"
          />
          <label for="10">x 10</label>
          <br />
          <input
            type="checkbox"
            id="9"
            :value="currRowPicks.n9.nr"
            v-model="currRowPicks.n9.checked"
          />
          <label for="9">x 9</label>
          <br />
          <input
            type="checkbox"
            id="8"
            :value="currRowPicks.n8.nr"
            v-model="currRowPicks.n8.checked"
          />
          <label for="8">x 8</label>
          <br />
          <input
            type="checkbox"
            id="7"
            :value="currRowPicks.n7.nr"
            v-model="currRowPicks.n7.checked"
          />
          <label for="7">x 7</label>
          <br />
          <input
            type="checkbox"
            id="6"
            :value="currRowPicks.n6.nr"
            v-model="currRowPicks.n6.checked"
          />
          <label for="6">x 6</label>
          <br />
          <input
            type="checkbox"
            id="5"
            :value="currRowPicks.n5.nr"
            v-model="currRowPicks.n5.checked"
          />
          <label for="5">x 5</label>
          <br />
          <input
            type="checkbox"
            id="4"
            :value="currRowPicks.n4.nr"
            v-model="currRowPicks.n4.checked"
          />
          <label for="4">x 4</label>
          <br />
          <input
            type="checkbox"
            id="3"
            :value="currRowPicks.n3.nr"
            v-model="currRowPicks.n3.checked"
          />
          <label for="3">x 3</label>
          <br />
          <input
            type="checkbox"
            id="2"
            :value="currRowPicks.n2.nr"
            v-model="currRowPicks.n2.checked"
          />
          <label for="2">x 2</label>
          <br />
          <input
            type="checkbox"
            id="1"
            :value="currRowPicks.n1.nr"
            v-model="currRowPicks.n1.checked"
          />
          <label for="1">x 1</label>
          <br />
        </div>
        <div>
          <transition>
            <span v-if="hint">🚫</span>
          </transition>
          <button @click.prevent="saveSettings">👍</button>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<script>
export default {
  beforeMount() {
    this.currMultipliers = this.multipliers;
    this.currRows = this.rows;
    this.currRowPicks = this.rowPicks;
  },
  emits: ["close-settings", "set-settings"],
  inject: ["multipliers", "rows", "rowPicks"],
  data() {
    return {
      all12: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
      all10: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      currMultipliers: null,
      currRows: null,
      currRowPicks: null,
      hint: false,
    };
  },
  methods: {
    closeSettings() {
      this.$emit("close-settings");
    },
    saveSettings() {
      this.currRows = [];
      for (const value of Object.values(this.currRowPicks)) {
        if (value.checked) {
          this.currRows.push(value.nr);
        }
      }
      this.$emit(
        "set-settings",
        this.currMultipliers,
        this.currRows,
        this.currRowPicks
      );
      if (this.currRows.length >= 1) {
        this.hint = false;
        this.closeSettings();
      } else {
        this.hint = true;
      }
    },
  },
};
</script>

<style scoped>
.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.25);
  z-index: 10;
}
.popUp {
  border-radius: 4px;
  position: fixed;
  top: 5vh;
  left: 40%;
  width: 20%;
  z-index: 100;
  border: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  margin: 0;
  overflow: hidden;
  background-color: white;
  max-height: 800px;
  overflow: auto;
  font-size: 1.5rem;
  line-height: 1.5;
}
menu {
  float: right;
  margin: 0;
  font-size: 1rem;
}

button {
  border: solid 1px lightgrey;
  margin: 0.5rem;
  padding: 0.5rem;
  width: 4rem;
  font-size: 2rem;
  border-radius: 20px;
  cursor: pointer;
  float: right;
  background-color: white;
  transition: 0.5s;
}

button:hover {
  background-color: lightgrey;
  transition: 0.5s;
}

.multipliers {
  margin-bottom: 2rem;
}

input {
  width: 1.5rem;
  height: 1.5rem;
  vertical-align: -0.3rem;
  margin-right: 0.5rem;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.settings-enter-active,
.settings-leave-active {
  transition: opacity 0.5s ease;
}

.settings-enter-from,
.settings-leave-to {
  opacity: 0;
}

@media only screen and (max-width: 560px) {
  .popUp {
    left: 5%;
    min-width: 0;
    width: 80%;
  }
}
</style>
