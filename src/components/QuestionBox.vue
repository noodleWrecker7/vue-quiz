<template>
  <div class="question-box-container">
    <div class="question-box">
      <h6>{{ this.question ? decodeString(this.question.question) : "" }}</h6>

      <hr>
      <p v-for="answer in listOfAnswers" :key="answer">{{ answer }}</p>

      <div>
        <button class="cool-button submit">Submit</button>
        <button @click="nextQuestion" class="cool-button next">Next</button>
      </div>

    </div>
  </div>
</template>

<script>


export default {
  name: "QuestionBox",
  props: {
    'question': Object,
    'nextQuestion': Function
  },
  computed: {
    listOfAnswers: function () {
      if (!this.question) return null;
      let list = this.question.incorrect_answers;
      let r = Math.floor(Math.random() * 3);
      list.splice(r, 0, this.question.correct_answer);
      return list;
    }
  },
  methods: {
    decodeString(str) {
      let d = document.createElement("p");
      d.innerHTML = str;
      return d.innerText;
    }
  }
}
</script>

<style scoped>
.submit {
  background: dodgerblue;

}

.next {
  background: limegreen;
}

.cool-button:hover {
  opacity: 0.6;
  transition: linear opacity 0.2s;
}

.cool-button {
  color: white;
  border-radius: 5px;
  margin-right: 1vw;
  padding: 1em;
  font-family: Avenir, sans-serif;
  border: none;
}

h6 {
  font-size: 3vh;
  margin: 1em
}

.question-box-container {
  width: 100%;
  /*background: linear-gradient(to bottom right, #fa52ca, #d89eef);*/
  background: aliceblue;
  padding: 3%;
  border-radius: 20px;
  border: solid grey 2px;
  min-height: 50vh;
}
</style>