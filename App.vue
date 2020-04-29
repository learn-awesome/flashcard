<template>
  <div id="app">
    <p>
      This is a set of work-in-progress components that can help
      anyone turn their articles into
      <a
        href="https://quantum.country/"
        target="_blank"
      >mnemonic media like this</a>. The code is
      <a
        href="https://github.com/learn-awesome/flashcard"
        target="_blank"
      >available on Github</a>.
      <br>
      <br>In your webpage, you would use the
      <code>PracticeSet</code> component specifying
      a bunch of question-answer pairs, and it will use localStorage to store
      user's recall performance in order to determine practice frequency.
    </p>
    <h3>A single flashcard</h3>
    <FlashCard qid="qid1" debug="off">
      <template v-slot:question>When did India become independent?</template>
      <template v-slot:answer>1947</template>
    </FlashCard>

    <br>
    <br>
    <h3>A set of flashcards to practice</h3>
    <PracticeSet debug="off" showskip="off">
      <FlashCard qid="qid1" debug="off">
        <template v-slot:question>When did India become independent?</template>
        <template v-slot:answer>1947</template>
      </FlashCard>
      <FlashCard qid="qid2" debug="off">
        <template v-slot:question>How many countries in UN security council?</template>
        <template v-slot:answer>5</template>
      </FlashCard>
      <FlashCard qid="qid3" debug="off">
        <template v-slot:question>In CSS, flex is the value of which property?</template>
        <template v-slot:answer>display</template>
      </FlashCard>
      <FlashCard qid="qid4" debug="off">
        <template v-slot:question>COVID-19 is caused by which family of viruses?</template>
        <template v-slot:answer>Coronavirus</template>
      </FlashCard>
    </PracticeSet>
  </div>
</template>

<script>
import FlashCard from "./components/FlashCard.vue";
import PracticeSet from "./components/PracticeSet.vue";

export default {
  components: {
    FlashCard,
    PracticeSet
  }
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
}

#app {
  max-width: 960px;
  margin: 0 auto;
  line-height: 1.4;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h1 {
  text-align: center;
}
</style>
