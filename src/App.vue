<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="handleBeforeStart($event)"/>
  <interac-screen v-if="statusMatch === 'match'" 
  :cardsContext="settings.cardsContext"
  @onFinish="onGetResult"
  />
  <result-screen v-if="statusMatch === 'result'"
  :timer="timer"
  @onStartAgain="statusMatch = 'default'"/>
</template>

<script>
import MainScreenVue from './components/MainScreen.vue';
import InteracScreenVue from './components/InteracScreen.vue';
import ResultScreenVue from './components/ResultScreen.vue';

import {shuffle}  from './utils/array.js';

export default {
  name: 'App',
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    }
  },
  components: {
    mainScreen: MainScreenVue,
    interacScreen: InteracScreenVue,
    resultScreen: ResultScreenVue,
  },
  methods: {
    handleBeforeStart(config) {
      console.log("handleBeforeStart App.vue", config);
      this.settings.totalOfBlock = config.totalOfBlock;
      const firstCards = Array.from({length: config.totalOfBlock / 2}, (_,i) => i + 1);
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffle(shuffle(shuffle(shuffle(cards))));
      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime()- this.settings.startedAt;
      this.statusMatch = "result";
    }
  }
}
</script>

<style>

</style>
