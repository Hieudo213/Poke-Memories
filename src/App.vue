<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="setting.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";
import { shuffled } from "./utils/array";
export default {
  name: "App",
  components: { MainScreen, InteractScreen, ResultScreen, CopyRight },
  data() {
    return {
      setting: {
        totalBlock: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      this.setting.totalBlock = config.totalBlocks;
      const firstCards = Array.from(
        { length: this.setting.totalBlock / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      // data ready
      this.setting.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));
      this.setting.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.setting.startedAt;
      // switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
