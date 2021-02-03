<template>
  <div id="app">
    <Header></Header>
    <QuestionBox id="qbox" :question="questions[index]" :next-question="nextQuestion"></QuestionBox>
  </div>
</template>

<script>
import Header from "@/components/Header";
import QuestionBox from "@/components/QuestionBox";

export default {
  name: 'App',
  data(){
    return {
      questions: [],
      index: 0
    }
  },
  methods:{
    nextQuestion(){
      this.index ++;
    }
  },
  components: {
    Header,
    QuestionBox
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    }).then((response) => {
      return response.json();
    }).then((jsonData) => {
      this.questions = jsonData.results;
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
