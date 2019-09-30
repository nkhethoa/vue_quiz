<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>
      </template>

      <hr class="my-4">

      <b-list-group v-for="(ans, key) in answer" :key="key">
        <b-list-group-item 
          v-html="ans" 
          @click="selectedAns(key)"
          :class="answerClass(key)"
          >
        </b-list-group-item>
      </b-list-group>

      <b-button  
          @click="submitAnswer"
          :disabled="selectedValue === null || answered" 
          variant="primary">Submit
      </b-button>
      <b-button  
        @click="next"
        variant="success">Next
      </b-button>

    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return{
      selectedValue : null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
    
  },
  computed: {
    answer() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      // answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.answered = false
        this.selectedValue = null
        this.shuffleAnswers()
      }
    }

  },
  //----type 1 watch
  // watch: {
  //   currentQuestion() {
  //     this.selectedValue = null
  //     this.shuffleAnswers()
  //   }
  // },
  methods: {
    selectedAns(value) {
      this.selectedValue = value

    },

    submitAnswer() {
      let isCorrect = false

      if(this.selectedValue === this.correctIndex){
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)

    },

    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },

    answerClass(key) {
      if (!this.answered && this.selectedValue === key) {
        return 'selected'
      }else if (this.answered && this.correctIndex === key) {
        return 'correct'
      } else if (this.answered 
          && this.selectedValue === key 
          && this.correctIndex !== key) {
        return 'incorrect'
      } else {
        return ''
      }

    }
  }
}
</script>

<style scoped>
  .list-group{
    margin-bottom: 5px
  }
  .list-group-item:hover{
    background-color: #ddd;
    color: #000
  }
  .btn {
    margin: 10px 5px
  }
  .selected{
    background-color: #251f7a;
    color:#fff
  }
  .correct{
    background-color: #048821;
    color:#fff
  }
  .incorrect{
    background-color: #880707;
    color:#fff
  }
</style>