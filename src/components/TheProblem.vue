<template>
  <the-popup v-if="catReward" @close-popup="closePopup"></the-popup>
  <div v-if="problemSet" class="catmatapp">
    <div class="problem">
      <span class="first">{{ first }}</span
      ><span class="operation">{{ operator.symbol }}</span
      ><span class="second">{{ second }}</span>
    </div>
    <div v-if="!hint" class="input">
      <input
        id="catput"
        type="text"
        v-model="input"
        @keyup.enter="validateInput"
        autoComplete="off"
        autofocus
      />
    </div>
    <div v-if="hint" class="hint">
      <div>
        🙀 {{ first }} {{ operator.symbol }} {{ second }} = {{ result }}
      </div>
      <div>
        <button class="resetButton" @click.prevent="resetProblem">🔁</button>
      </div>
    </div>
    <div v-if="!hint" class="time">
      <progress :value="time" :max="timeLimit"></progress><span id="timer">⏲️</span>  
    </div>
    <div v-if="!hint" class="progress">
      <progress :value="solved" :max="toSolve"></progress><span id="paws">🐾</span>
    </div>
  </div>
</template>

<script>
import ThePopup from "./ThePopup.vue";

export default {
  components: {
    ThePopup
  },
  mounted() {
    this.setProblem();
  },
  props: ["mode", "multipliers", "rows"],
  inject: ["toSolve", "timeLimit"],
  data() {
    return {
      operators: [
        { mode: 1, symbol: "·" },
        { mode: 2, symbol: ":" },
      ],
      first: null,
      operator: null,
      second: null,
      result: null,
      input: null,
      problemSet: false,
      solved: 0,
      hint: false,
      timer: null,
      time: 0,
      catReward: false
    };
  },
  methods: {
    async setProblem() {
      this.setHint(false);
      this.input = null;
      if (this.mode === 3) {
        this.operator =
          this.operators[Math.floor(Math.random() * this.operators.length)];
      } else {
        this.operator = this.operators[this.mode - 1];
      }
      const multiplier =
        this.multipliers[Math.floor(Math.random() * this.multipliers.length)];
      const multiplicant =
        this.rows[Math.floor(Math.random() * this.rows.length)];
      const product = multiplier * multiplicant;
      if (this.operator.mode === 1) {
        // multiplication
        this.first = multiplier;
        this.second = multiplicant;
        this.result = product;
      } else {
        // division
        this.first = product;
        this.second = multiplicant;
        this.result = multiplier;
      }
      this.problemSet = true;
      this.startTimer()
    },
    async setHint(boolean) {
      this.hint = boolean;
    },
    setFocus() {
      document.getElementById("catput").focus();
    },
    async resetProblem() {
      this.stopTimer();
      await this.setHint(false);
      this.setFocus();
      this.setProblem();
    },
    validateInput() {
      if (this.input == this.result) {
        this.solved += 1;
        this.setProblem();
      } else {
        this.setHint(true);
        this.solved = 0;
        this.stopTimer();
      }
    },
    addTime(increase) {
      this.time += increase;
      console.log(this.time);
    },
    startTimer() {
      this.time = 0;
      if (this.timer) {
        this.stopTimer();
      }
      this.timer = setInterval(this.addTime, 1000, 1)
    },
    stopTimer() {
      clearInterval(this.timer);
      this.time = 0;
    },
    async closePopup() {
      this.catReward = false;
      await this.setProblem();
      this.setFocus();
    }
  },
  watch: {
    solved(newValue) {
      if (newValue === this.toSolve) {
        this.problemSet = false;
        this.stopTimer();
        this.solved = 0;
        this.catReward = true;
      }
    },
    time(newValue) {
      if (newValue === this.timeLimit) {
        this.stopTimer();
        this.setHint(true);
        this.solved = 0;
      }
    }
  },
};
</script>

<style scoped>
.catmatapp {
  text-align: center;
}

.problem {
  font-size: 6rem;
}

#catput {
  width: 100px;
  padding: 20px;
  text-align: center;
  font-size: 2rem;
  border-radius: 20px;
}

.hint {
  margin-top: 2rem;
  font-size: 2rem;
}

.time {
  margin-top: 2rem;
}

.progress {
  margin-top: 0.5rem;
}

progress {
  width: 75%;
  height: 50px;
  max-width: 400px;
}

#timer, #paws {
  padding-left: 0.5em;
  vertical-align: .4em;
  font-size: 2rem;
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
