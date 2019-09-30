<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTot="numTot"
     />
    <b-container class="bv-example-row bv-example-row-flex-cols mt-3">
    <b-row>
      <b-col md="6" offset-md="3">
        <QuesBox 
          v-if="questions.length > 0"
          :currentQuestion="questions[index]"
          :next="next"
          :increment="increment"
         />
      </b-col>
    </b-row>
</b-container>
  
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuesBox from './components/QuesBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuesBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect:0,
      numTot:0
    }
  },
  methods:{
    next(){
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
        this.next()
      }
      this.numTot++
    }
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=26&difficulty=easy&type=multiple', {
      method: 'get'
    })
    .then((res) => {
      return res.json()
    })
    .then((jsonData) => {
      this.questions = jsonData.results
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{
  background-color: #d3d3d3
}
</style>
