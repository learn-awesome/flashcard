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
    <slot>VueJS WebComponents not supported in this browser.</slot>
    <div>
      <p v-show="getCards().length === 0">No FlashCards remaining in this set.</p>
      <button v-show="showskip === 'on' && getCards().length > 1" v-on:click="skip()">Skip</button>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    debug: {
      type: String,
      required: false
    },
    showskip: {
      type: String,
      required: false
    },
    childTagName: {
      type: String,
      default: "FlashCard"
    }
  },

  methods: {
    onAnswered: function(qid, answer) {
      console.log(qid, answer);
      // assuming that only one card is displayed at a time, so qid is the first
      if (answer === "recalled") {
        this.removeFirstCard();
      } else {
        // forgot, so need to practice right away
        this.skip();
      }
    },
    skip: function() {
      // pop the first card and append it at the last
      let firstCard = this.$children.shift();
      if(firstCard){
        firstCard.makeUnanswered();
        this.$children.push(firstCard);
      }
      this.setChildVisibility();
      this.$forceUpdate();
    },
    getCards: function() {
      const cards = this.$children.filter(
        child =>
          child.$vnode.tag.match(`^vue-component-\\d+-${this.childTagName}$`) !==
          null
      );
      return cards;
    },

    removeFirstCard: function(){
      let card = this.getCards()[0];
      if(card) {
        card.$el.style.display = 'none'; // This shouldn't be necessary if we're going to destroy the first child, but somehow is.
        card.$destroy();
      }
      this.setChildVisibility();
      this.$forceUpdate();
    },

    setChildVisibility(){
      // console.log("We only want to show the first flashcard");
      this.getCards().forEach(function(card,index){
        if(index > 0)
          card.$el.style.display = 'none';
        else
          card.$el.style.display = 'block';
      });
    }
  },
  computed: {},
  mounted() {
    this.setChildVisibility();
    this.$forceUpdate();
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
  display: block;
  margin: 0 auto;
  text-align: center;
}

.practiceset .flash-card {
  display: none;
}

.practiceset .flash-card:first-child {
  display: block;
}
</style>