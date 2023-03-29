<template>
  <div class="">
    <main-screen
      v-if="statusMatch === 'default'"
      @onStart="onHandleBeforeStart($event)"
    />
    <interact-screen
      v-if="statusMatch === 'match'"
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
    />
    <result-screen
      v-if="statusMatch === 'result'"
      :timer="timer"
      @onStartAgain="statusMatch = 'default'"
    />
    <coppy-right />
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CoppyRight from "./components/CoppyRightScreen.vue";

import { shuffled } from "./utils/array.js";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CoppyRight,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("Running handle before start", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firtCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      // Coppy mảng bằng ES6
      const secondCards = [...firtCards];
      const cards = [...firtCards, ...secondCards];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      // console.log(firtCards);
      this.settings.startedAt = new Date().getTime();
      // console.log(secondCards);
      console.log(cards);
      // console.log(this.settings.cardContext);

      // data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      // switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
