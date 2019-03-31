<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">{{ currentQuestion.question }}</template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        v-on:click="submitAnswer"
        :disabled="selectedIndex === null"
      >Submit</b-button>
      <b-button v-on:click="next" variant="success">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      suffledAnswers: []
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    // Watch will run whenever props changes
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.suffleAnswers();
      }
    }
    // currentQuestion() {
    //   this.selectedIndex = null;
    //   this.suffleAnswers();
    // }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
    },
    suffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.suffledAnswers = _.shuffle(answers);
    }
  }
};
</script>

<style scoped>
.jumbotron {
  min-height: 600px;
}
.list-group {
  margin-bottom: 20px;
}
.list-group-item:hover {
  cursor: pointer;
  background: #e6e6e6;
}
.btn-primary {
  margin-right: 20px;
}
.selected {
  background: lightblue;
  color: #ffffff;
}
.correct {
  background: lightgreen;
}
.incorrect {
  background: red;
}
</style>
