<script lang="js">

import MainScreen from '@/components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import Copyright from './components/CopyRight.vue'
import { shuffled } from './utils/array'
import ResultScreen from './components/ResultScreen.vue'
// eslint-disable-next-line no-undef

export default {
  name: 'App',
  components: { MainScreen, InteractScreen, ResultScreen, Copyright },
  data() {
    return {
      settings: {
        totalBlocks: 0,
        cardsContext: [],
        startedAt: 0
      },
      statusMatch: 'default',
      timer: 0
    }
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log(config)
      this.settings.totalBlocks = config.totalBlocks
      const firstCards = Array.from({ length: config.totalBlocks / 2 }, (_, i) => i + 1)
      const secondCards = [...firstCards]
      const cards = [...firstCards, ...secondCards]
      this.settings.cardsContext = shuffled(cards)
      this.settings.startedAt = new Date().getTime()
      this.statusMatch = 'match'
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt
      this.statusMatch = 'result'
    }
  }
}
</script>

<template>
  <MainScreen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)" />
  <interact-screen
    @onFinish="onGetResult"
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <Copyright />
</template>

<style scoped></style>
