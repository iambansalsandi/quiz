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

      <b-button variant="primary">Submit</b-button>
      <b-button v-on:click="next" variant="success">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null
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
    currentQuestion() {
      this.selectedIndex = null;
      this.suffleAnswers();
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    suffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
    }
  },
  mounted() {
    console.log(this.currentQuestion);
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
