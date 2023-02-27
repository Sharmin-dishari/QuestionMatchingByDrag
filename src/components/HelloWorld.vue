<template>
  <div class="row" style="display: flex; flex-direction: row">
    <div class="column right">
      <div>
        <div
          v-for="(question, index) in questions"
          :key="index"
          v-bind:class="{ matched: matches[index] !== null }"
          @drop="drop(index)"
          @dragover.prevent
        >
          <div class="row">
            <div class="column left card cardColr">
              {{ question.name }}
            </div>
            <div
              class="column right card"
              style="margin-left: 10px"
              v-bind:draggable="true"
              @dragstart="undoMatch(index)"
              v-if="matches[index] !== null"
            >
              {{ matches[index].name }}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="column left" style="margin-left: 30px"></div>
    <div class="column center" style="">
      <div
        v-for="(answer, index) in answers"
        :key="index"
        v-bind:draggable="true"
        class="card all-scroll"
        @dragstart="dragStart(index)"
      >
        <div v-if="!matchedAnswers.includes(answer)">
          {{ answer.name }}
        </div>
        <span v-else class="placeholder"></span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data: function () {
    return {
      questions: [
        {
          id: 1,
          name: "Hi, Georgie.It's Simon",
        },
        {
          id: 2,
          name: "How about that new pub in Covent Garden - The Fallen Angel?",
        },
        {
          id: 3,
          name: "Fine. I was wondering if you were doing anything tomorrow evening.",
        },
        {
          id: 4,
          name: "Oh right. What are you going to see?",
        },
        {
          id: 5,
          name: "Right, well, I guess you're busy then",
        },
        {
          id: 6,
          name: "OK, well, do you fancy going for a drink afterwards?",
        },
      ],
      answers: [
        {
          id: 1,
          name: "Oh, hi Simon how are you",
        },
        {
          id: 2,
          name: "Tomorrow, Er, let me think, oh yes I'm goint an art exhibition after work",
        },
        {
          id: 3,
          name: "Fine. I was wondering if you were doing anything tomorrow evening.",
        },
        {
          id: 4,
          name: "Oh right. What are you going to see?",
        },
        {
          id: 5,
          name: "Right, well, I guess you're busy then",
        },
        {
          id: 6,
          name: "OK, well, do you fancy going for a drink afterwards?",
        },
      ],
      matches: [null, null, null, null, null, null],
      matchedAnswers: [],
    };
  },
  methods: {
    dragStart(index) {
      this.matchedIndex = index;
    },
    dropQuestion(index) {
      const questionIndex = event.dataTransfer.getData("text/plain");
      const question = this.matchedAnswers.splice(questionIndex, 1)[0];
      this.matchedAnswers.splice(index, 0, question);
    },
    drop(index) {
      if (this.matches[index] !== null) {
        // If the question already has a match, remove it
        const matchedAnswer = this.matches[index];
        const matchedAnswerIndex = this.matchedAnswers.indexOf(matchedAnswer);
        this.matchedAnswers.splice(matchedAnswerIndex, 1);
      }

      // Set the new match and update the matchedAnswers list
      this.matches.splice(index, 1, this.answers[this.matchedIndex]);
      this.matchedAnswers.push(this.answers[this.matchedIndex]);
      this.matchedIndex = null;
    },
    undoMatch(index) {
      const matchedAnswer = this.matches[index];
      const matchedAnswerIndex = this.matchedAnswers.indexOf(matchedAnswer);
      this.matches.splice(index, 1, null);
      this.matchedAnswers.splice(matchedAnswerIndex, 1);
    },
  },
};
</script>
<style>
.column {
  float: left;
  padding: 10px;
}

.left {
  width: 8%;
}
.all-scroll {
  cursor: all-scroll;
}
.right {
  width: 40%;
  margin-left: 100px;
}
.center {
  width: 20%;
}
.row:after {
  content: "";
  display: table;
  clear: both;
}

.card {
  padding: 45px;
  border: 1px solid black;
  margin-bottom: 25px;
  border-radius: 15px;
  max-width: 230px;
  min-height: 40px;
}
.cardColr {
  background-color: #eef7fc;
}
.container {
  padding: 2px 16px;
}
.placeholder {
  display: inline-block;
  border: 1px solid #ccc;
  margin-left: 5px;
}
@media only screen and (width> 1300px) {
  .left {
    width: 25%;
  }
  .card {
    width: 230px;
    min-height: 40px;
  }
  .right {
    width: 50%;
    margin-left: 100px;
  }
  .center {
    width: 20%;
  }
}
</style>
