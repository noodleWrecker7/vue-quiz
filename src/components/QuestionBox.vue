<template>
  <div class="question-box-container">
    <div class="question-box">
      <h6 @change="updateData">{{ this.question ? decodeString(this.question.question) : "" }}</h6>

      <hr>
      <p @click="answerClicked(index)" v-for="(answer, index) in listOfAnswers" :key="index"
         :class="{selected: selectedIndex == index, correct: index == revealCorrectIndex, incorrect: index == selectedIndex && revealCorrectIndex != -1}">
        {{ decodeString(answer) }}
      </p>

      <div>
        <button @click="prevQuestion" class="cool-button prev">Previous</button>
        <button @click="submitAnswer" class="cool-button submit">Submit</button>
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
    'nextQuestion': Function,
    'prevQuestion': Function,
    'questionAnswered': Function

  },
  data() {
    return {
      correctIndex: -1,
      selectedIndex: -1,
      revealCorrectIndex: -1
    }
  },
  computed: {
    /*selectedIndex: function(){
      return this.question.selectedIndex;
    },*/
    listOfAnswers: function () {
      if (!this.question) return null;
      let list = [...this.question.incorrect_answers];
      let r;
      if (this.question.correctIndex >= 0) {
        r = this.question.correctIndex;
      } else {

        r = Math.floor(Math.random() * 3);
        this.storeCorrectIndex(r);
      }
      list.splice(r, 0, this.question.correct_answer);
      return list;
    }
  },
  updated() {
    this.updateData();
  },
  methods: {
    updateData() {
      this.selectedIndex = this.question.selectedIndex;
      this.revealCorrectIndex = this.question.revealCorrectIndex;
    },
    submitAnswer() {
      if (this.selectedIndex == -1) return;
      if (this.revealCorrectIndex != -1) return;
      this.question.revealCorrectIndex = this.question.correctIndex;
      this.revealCorrectIndex = this.question.revealCorrectIndex;
      if (this.selectedIndex == this.revealCorrectIndex) {
        this.questionAnswered(true)
      } else {
        this.questionAnswered(false)
      }
    },

    answerClicked(el) {
      if (this.question.revealCorrectIndex >= 0) return;
      console.log("answer clicked")
      this.selectedIndex = el;
      this.question.selectedIndex = el;
    },
    storeCorrectIndex(i) {
      this.question.correctIndex = i;
    },
    decodeString(str) {
      let d = document.createElement("p");
      d.innerHTML = str;
      return d.innerText;
    }
  }
}
</script>

<style scoped>
.selected {
  border: solid grey 2px;
  color: black;
}

.incorrect {
  background: red;
  color: white;
  transition: linear 1s;
}

.correct {
  background: limegreen;
  color: black;
  transition: linear 1s;
}

.next, .prev {
  background: dodgerblue;

}

.submit {
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