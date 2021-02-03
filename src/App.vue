<template>
  <div id="app">
    <Header :index="index" :correct-count="correctCount" :incorrect-count="incorrectCount"></Header>
    <QuestionBox ref="qbox" v-if="questions.length" id="qbox" :question="questions[index]" :next-question="nextQuestion"
                 :prev-question="prevQuestion" :question-answered="questionAnswered"></QuestionBox>
  </div>
</template>

<script>
import Header from "@/components/Header";
import QuestionBox from "@/components/QuestionBox";

export default {
  name: 'App',
  data() {
    return {
      questions: [],
      index: 0,
      counter: 0,
      correctCount: 0,
      incorrectCount: 0
    }
  },
  methods: {
    questionAnswered(correct) {
      if (correct) this.correctCount++;
      else this.incorrectCount++;
    },
    nextQuestion() {
      this.index++;
      /*   this.$refs["qbox"].$forceUpdate();
         this.$refs["qbox"].updateData();*/
    },
    prevQuestion() {
      this.index--;
      /* parent.$refs.qbox.updateData();*/
    }

  },
  components: {
    Header,
    QuestionBox
  },
  mounted: function () {
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    }).then((response) => {
      return response.json();
    }).then((jsonData) => {
      this.questions = jsonData.results;
      for (let i = 0; i < this.questions.length; i++) {
        this.questions[i].selectedIndex = -1;
        this.questions[i].revealCorrectIndex = -1;
      }
    });
  }
}
</script>

<style>
#qbox {
  width: 40vw;
  margin-left: auto;
  margin-right: auto;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
