<template>
  <div id="app">
    <CardSet timeunit="seconds"></CardSet>
  </div>
</template>

<script>
import CardSet from "./components/CardSet.vue";

export default {
  components: {
    CardSet
  },
  data() {
    return {
      flashCards: [
          {qid: 'qid1', question: 'q1', answer: 'a1', flipped: false, answered: null},
          {qid: 'qid2', question: 'q2', answer: 'a2', flipped: false, answered: null},
          {qid: 'qid3', question: 'q3', answer: 'a3', flipped: false, answered: null}
      ]
    }
  }
};
</script>

<style>

</style>
