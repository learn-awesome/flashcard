<template>
  <div class="simpleset">
      <FlashCard v-for="card in cards.slice(0,1)" 
        :key="card.qid"
        :qid="card.qid"
        :question="card.question"
        :answer="card.answer"
        @answered=onAnswered
        @flipped=onFlipped
        :flipped="card.flipped"
        :answered="null"
        :timeunit="timeunit"
      />
      <p v-show="cards.length == 0" class="message">
      All done! 👍<br/>
      <span>Come back later to this page to practice again. Your progress is only kept on your own device.</span>
      </p>
  </div>
</template>

<script>
import FlashCard from './FlashCard.vue';

export default {
    components: {FlashCard},
    data(){
        return {
            cards: this.$parent.flashCards
        }
    },

    props: {
      timeunit: {
        type: String,
        required: true,
        default: "days"
      }
    },

  methods: {
    onFlipped: function(qid, is_flipped){
        //console.log("flipped: ", qid, is_flipped);
        let card = this.cards.find((c) => c.qid === qid);
        if(card) card.flipped = is_flipped;
    },
    onAnswered: function(qid, answer) {
      //console.log("onAnswered: ", qid, answer);
      let card = this.cards.find((c) => c.qid === qid);

      if (answer === "recalled") {
          // remove from the set
        this.cards.splice( this.cards.indexOf(card), 1 );
      } else {
        // forgot, so need to practice right away
        // remove and append at the end
        this.skip(card);
      }
    },
    skip: function(card) {
      // pop the first card and append it at the last
      this.cards.splice( this.cards.indexOf(card), 1 );
      card.answered = null;
      card.flipped = false;
      this.cards.push(card);
    }
  }
}
</script>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
}

.simpleset {
  display: block;
  margin: 0 auto;
  text-align: center;
  max-width: 960px;
}

.simpleset .flash-card {
  display: none;
}

.simpleset .flash-card:first-child {
  display: block;
}

p.message {
  margin: 0 16px;;
  border-radius: 3px;
  color: #222;
  background-color: #fcfaff;
  border: 2px solid purple;
  box-shadow: 3px 3px 20px rgb(70, 70, 196);
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  line-height: 1.4;
  max-width: 960px;
  padding: 2.0em;
  font-weight: bold;
  font-size: 3.0em;
  -webkit-font-smoothing: antialiased;
}

p.message span {
    font-weight: normal;
    font-size: 0.5em;
}
</style>