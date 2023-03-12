<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((720 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) *
          Math.sqrt(cardsContext.length)
        }px`
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRules($event)"
        :cardsContext="cardsContext"
      />
    </div>
  </div>
</template>
<script>
import CardFlip from './CardPoke.vue'
export default {
  data() {
    return {
      rules: []
    }
  },
  methods: {
    checkRules(card) {
      if (this.rules.length === 2) return false
      this.rules.push(card)

      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        console.log('Right')
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode()
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode()
        this.rules = []
        const disabledElement = document.querySelectorAll('.screen .card.disabled')
        if (disabledElement && disabledElement.length === this.cardsContext.length - 2) {
          setTimeout(() => {
            this.$emit('onFinish')
          }, 920)
        }
      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        // console.log(this.rules)
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard()
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard()
          this.rules = []
        }, 800)
      } else return false
    }
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return []
      }
    }
  },

  components: { CardFlip }
}
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}
.screen__inner {
  margin: 2rem auto;
  display: flex;
  flex-wrap: wrap;
}
</style>
