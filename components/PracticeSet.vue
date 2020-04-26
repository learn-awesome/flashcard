<!--

Overview:

This represents an array of flashcards that are currently selected for practice set.
The flashcards are specified via data object. Only the first card in the array
gets displayed. If the card emits 'answered' event, we remove that card from the
current set, otherwise, it just gets appended to the end of the list.

debug: on|off. Set via props and gets passed on to each of the cards
showskip: on|off. Enables skip functionality

TODO:
- Allow passing FlashCards list via slots
- Publish both components as a single JS file on CDN

-->
<template>
  <div class="practiceset">
    <FlashCard
      v-for="fc in firstCard"
      :key="fc.qid"
      :qid="fc.qid"
      :question="fc.question"
      :answer="fc.answer"
      :debug="debug"
      @answered="onAnswered(fc.qid, $event)"
    />
    <div v-if="firstCard.length === 0">You're all done for now. Good job! :-)</div>
    <button v-if="showskip === 'on' && firstCard.length > 0" v-on:click="skip()">Skip</button>
  </div>
</template>

<script>
import FlashCard from "./FlashCard.vue";

export default {
  props: {
    debug: {
      type: String,
      required: false
    },
    showskip: {
      type: String,
      required: false
    }
  },
  components: { FlashCard },
  data: function() {
    return {
      flashcards: [
        {
          qid: "qid1",
          question: "When did India become independent?",
          answer: "1947"
        },
        {
          qid: "qid2",
          question: "How many countries in UN security council?",
          answer: "5"
        },
        {
          qid: "qid3",
          question: "In CSS, flex is the value of which property?",
          answer: "display"
        },
        {
          qid: "qid4",
          question: "COVID-19 is caused by which family of viruses?",
          answer: "Coronavirus"
        }
      ]
    };
  },
  methods: {
    onAnswered: function(qid, answer) {
      console.log(qid, answer);
      // assuming that only one card is displayed at a time, so qid is the first
      if (answer === "recalled") {
        this.flashcards.shift();
      } else {
        // forgot, so need to practice right away
        this.skip();
      }
    },
    skip: function() {
      // pop the first card and append it at the last
      this.flashcards.push(this.flashcards.shift());
    }
  },
  computed: {
    firstCard: function() {
      var self = this;
      return self.flashcards.filter(function(fc, index) {
        return index === 0;
      });
    }
  }
};
</script>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
}

.practiceset {
  display: inline-block;
}
</style>