<template lang="html">
  <div class="quiz-navigation">
    <b-row class="d-none d-md-block" v-bind:key="question.id">
      <b-col class="my-3 text-center">
        <b-button-group class="mx-1">
          <b-button
            :disabled="! prevEnabled"
            @click.stop="prevQuestion"
          >‹ {{ $t('nav.prev_question') }}
          </b-button>
        </b-button-group>
        <b-button-group class="mx-1">
          <b-button
            :disabled="! nextEnabled"
            @click.stop="nextQuestion"
          >{{ $t('nav.next_question') }} ›
          </b-button>
        </b-button-group>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: [
    'question'
  ],
  computed: {
    ...mapGetters({
      getSelectedAnswerValue: 'quiz/getSelectedAnswer',
      currentQuestionNumber: 'quiz/currentQuestionNumber'
    }),
    nextEnabled () {
      if (this.question.type === 'info') return true
      return this.getSelectedAnswerValue(this.question.id) !== undefined
    },
    prevEnabled () {
      return this.currentQuestionNumber > 1
    }
  },
  methods: {
    nextQuestion () {
      this.$store.commit('quiz/setQuestionNumber', this.currentQuestionNumber + 1)
      this.$store.commit('quiz/updateCurrentQuestion')
    },
    prevQuestion () {
      this.$store.commit('quiz/setQuestionNumber', this.currentQuestionNumber - 1)
      this.$store.commit('quiz/updateCurrentQuestion')
    }
  }
}
</script>

<style lang="css">
</style>
