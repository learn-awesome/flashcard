<!-- 

Overview

Every flashcard has these attributes:
  qid: per-site unique identifier used for localStorage lookups, only passed via props
  debug: on|off, only passed via props. Enables more logging & detailed messaging in UI.
  question, answer: String/HTML. Passed via either props or slots (see PracticeSet.vue for example)
  flipped: state variable that reveals the answer. Never persisted.
  answered: state variable that shows a message after answering. Never persisted.
  answer: forgot|recalled
  level: 1-10. Stored in localStorage with key as <qid>_level
  practice_timestamp: Last time this question was practiced
  sucess_timestamp: Last time this question was successfully recalled

Level determines when this flashcard will be selected for practice and if
successfully recalled, when the level will be allowed to increment. If the user is
able to successfully recall the answer after 2^level days, level gets incremented
If the level reaches 11, this card won't be selected for practice at all.

TODO:
- Test the level incrementing logic
- Handle localStorage edge cases
- Polish the design via CSS
- Combined different localStorage items into a single serialized JSON object
-->

<template>
  <div class="flashcard" v-on:click="flip()" :style="{cursor: flipped ? 'default' : 'pointer'}">
    <div class="question">
      <slot name="question"></slot>
    </div>

    <div class="answer">
      <slot v-if="flipped" name="answer"></slot>
      <div v-show="!flipped" class="tip">Recall the answer and click to reveal.</div>
    </div>

    

    <div class="showlevel">
      <p v-bind:class="{ active: (level == 1) }" title="Every 1 day">1</p>
      <p v-bind:class="{ active: (level == 2) }" title="Every 2 day">2</p>
      <p v-bind:class="{ active: (level == 3) }" title="Every 4 day">3</p>
      <p v-bind:class="{ active: (level == 4) }" title="Every 8 day">4</p>
      <p v-bind:class="{ active: (level == 5) }" title="Every 16 day">5</p>
      <p v-bind:class="{ active: (level == 6) }" title="Every 1 month">6</p>
      <p v-bind:class="{ active: (level == 7) }" title="Every 2 months">7</p>
      <p v-bind:class="{ active: (level == 8) }" title="Every 4 months">8</p>
      <p v-bind:class="{ active: (level == 9) }" title="Every 8 months">9</p>
      <p v-bind:class="{ active: (level == 10) }" title="Every 16 months">10</p>
      <p v-bind:class="{ active: (level == 11) }" title="Long-term">11</p>
    </div>

    <div :style="{visibility: flipped ? 'visible' : 'hidden'}" class="actions">
      <button
        v-if="answered == null"
        v-on:click="recall('forgot')"
        class="forgotbutton"
      >Didn't recall</button>
      
      <button
        v-if="answered == null"
        v-on:click="recall('recalled')"
        class="recalledbutton"
      >Recalled</button>
      
      

      <div v-if="answered" class="message">{{ message }}</div>
    </div>

    <button v-if="debug === 'on'" v-on:click.stop="reset()" class="resetbutton">Reset</button>

    <div v-if="debug === 'on'" class="debug-message">{{ debugMessage }}</div>
  </div>
</template>

<script>
export default {
  props: {
    qid: {
      type: String,
      required: true
    },
    question: {
      type: String,
      required: false // can be passed via slot
    },
    answer: {
      type: String,
      required: false // can be passed via slot
    },
    debug: {
      type: String,
      required: false // default will be taken as false
    }
  },
  data: function() {
    var level_string = window.localStorage.getItem(this.qid + "_level");
    return {
      flipped: false, // step-1 to step-2
      answered: null, // step-2 to step-3. Value is forgot|recalled
      level: level_string ? parseInt(level_string, 10) : null,
      practice_timestamp: window.localStorage.getItem(this.qid + "_pts"),
      success_timestamp: window.localStorage.getItem(this.qid + "_sts")
    };
  },
  methods: {
    flip: function() {
      if(this.flipped) return;
      this.flipped = true;

      // set last_practiced_timestamp
      var current_date = new Date();
      window.localStorage.setItem(
        this.qid + "_pts",
        current_date.toISOString()
      );
      this.practice_timestamp = current_date.toISOString();

      if (!this.level) {
        // initialize level
        window.localStorage.setItem(this.qid + "_level", 1);
        this.level = 1;
      }
    },

    makeUnanswered: function() {
      // invoked from skip() in PracticeSet
      this.answered = null;
    },

    reset: function() {
      this.flipped = false;
      this.answered = null;
      window.localStorage.removeItem(this.qid + "_level");
      this.level = null;
      window.localStorage.removeItem(this.qid + "_pts");
      this.practice_timestamp = null;
      window.localStorage.removeItem(this.qid + "_sts");
      this.success_timestamp = null;
    },

    recall: function(value) {
      // value is either forgot or recalled
      var current_date = new Date();

      if (value === "recalled") {
        // check current date with last_success_timestamp
        // if the diff is more than 2^level days and value = 'recalled'
        // then increase level by 1 with max = 10

        // can be null but new Date(null) gives unix epoch which is perfect
        var previous_ts = window.localStorage.getItem(this.qid + "_sts");
        var previous_date = new Date(previous_ts);

        var diff_in_days = Math.floor(
          (current_date.getTime() - previous_date.getTime()) / 1000.0
        );
        if (diff_in_days > Math.pow(2, this.level - 1)) {
          // increment level
          this.level = Math.min(this.level + 1, 11);
          window.localStorage.setItem(this.qid + "_level", this.level);
          //alert("You reached level: " + this.level);
          // if (this.level === 11) alert("Congrats! You reached level 11! :-)");
        }

        // set last_success_timestamp. pracetice_timestamp was already set in flip()
        window.localStorage.setItem(
          this.qid + "_sts",
          current_date.toISOString()
        );
        this.success_timestamp = current_date.toISOString();
      } else {
        // Decrement level or reset all the way to 1
        this.level = 1; // Math.max(1, this.level - 1);
        window.localStorage.setItem(this.qid + "_level", this.level);
        //alert("Decrementing level to " + this.level);
      }

      this.answered = value;

      if(this.$parent && this.$parent.onAnswered)
        this.$parent.onAnswered(this.qid, value); // this is handled in PracticeSet.vue
    }
  },
  computed: {
    message: function() {
      if (this.answered === "forgot") return "Could not recall.";
      if (this.answered === "recalled") return "Good job!";
      return "";
    },
    debugMessage: function() {
      return (
        "qid = " +
        this.qid +
        ", level = " +
        this.level +
        ", pts = " +
        this.practice_timestamp +
        ", sts = " +
        this.success_timestamp
      );
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

.flashcard {
  display: inline-block;
  max-width: 600px;
  min-width: 450px;
  border-radius: 3px;
  color: #222;
  background-color: white;
  border: 2px solid purple;
  box-shadow: 3px 3px 20px rgb(70, 70, 196);
}

.question {
  padding: 12px;
  font-weight: bold;
  font-size: 2em;
  border-bottom: 2px;
  border-bottom: 1px solid #f6f0ff;
  text-align: center;
  padding-top: 40px;
  padding-bottom: 40px;
}

.answer {
  padding: 12px;
  background-color: #fcfaff;
  font-weight: bold;
  font-size: 2em;
  text-align: center;
  padding-top: 40px;
  padding-bottom: 40px;
  min-height: 200px;
  vertical-align: middle;
}

.tip {
  color: #bbb;
  font-weight: normal;
}

.message {
  font-size: 14px;
  text-align: center;
}

.actions {
  display:flex;
  justify-content: space-around;
  padding-top: 10px;
  margin-bottom: 10px;
}

button {
  display: inline-block;
  width: 40%;
  font-weight: bold;
  font-size: 16px;
  padding-top: 10px;
  padding-bottom: 10px;
  margin-bottom: 8px;
  border-radius: 8px;
  cursor: pointer;
  opacity: 0.8;
}

button:hover {
  opacity: 1.0;
  font-weight: bold;
}

button.forgotbutton {
  background-color: #F56565;
  color: #742A2A;
}

button.recalledbutton {
  background-color: #48BB78;
  color: #22543D;
}

button.resetbutton {
  width: 98%;
  background-color: #CBD5E0;
  color: "black";
}

div.debug-message {
  text-align: center;
}

div.showlevel {
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-around;
  background-color: #fcfaff;
  padding-bottom: 10px;
  border-bottom: 1px solid #f6f0ff;
}

div.showlevel p {
  display: inline-block;
  padding: 6px 6px;
  margin: 3px;
  opacity: 0.5;
  border: thin solid green;
  border-radius: 5px;
  width: 32px;
  height: 32px;
  text-align:center;
}

div.showlevel p.active {
  background-color: lime;
  font-weight: bold;
}
</style>