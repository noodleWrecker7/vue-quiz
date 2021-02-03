<template>
  <nav>
    <p>Fancy Schmancy Quiz</p>
    <p>Counter: {{ index + 1 }}/10</p>
    <p>Correct: {{ correctCount }}/{{ correctCount + incorrectCount }}</p>
  </nav>
</template>

<script>
export default {
  name: "Header",
  props: {
    'index': Number,
    'incorrectCount': Number,
    'correctCount': Number
  }
}
</script>

<style scoped>
nav {
  display: flex;
  flex-flow: row;
  align-content: space-around;
  justify-content: center;
  /*background: aliceblue;*/
  /*background: linear-gradient(to top left, #cd52fa, #d89eef);*/
  border-bottom: solid grey 1px;
  font-size: 2vh;
  margin-bottom: 7vh;

}

nav > p {
  margin-left: 5vw;
  margin-right: 5vw;
  font-family: Avenir, sans-serif;
  font-weight: 700;
}

</style>