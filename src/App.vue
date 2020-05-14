<template>
  <div id="app">
    <h1> Lilly Harrington Homework - SMIS</h1>
    <button @click="spelling = true; speech = false;">Spelling</button>
    <button @click="speech =true; spelling = false;">Speech</button>
    <div v-if="spelling || speech">
      <hr>
      <button type="button" @click="words = week28">Week 28</button>
      <button type="button" @click="words = week29">Week 29</button>
      <button type="button" @click="words = week30">Week 30</button>
      <button type="button" @click="words = week31">Week 31</button>
    </div>
    <hr>
    <Speech v-if="speech && words != null " :words="words"></Speech>
    <Spelling v-if="spelling && words !=null" :words="words"></Spelling>
  </div>
</template>

<script>
import Speech from './components/Speech.vue'
import Spelling from './components/Spelling.vue'

export default {
  name: 'App',
  components: {
    Speech,
    Spelling
  },
  data: () => ({
    spelling: false, 
    speech: false,
    words: null,
    week28: [
        {word: "the", correct: false, attempts: 0},
        {word: "like", correct: false, attempts: 0},
        {word: "after", correct: false, attempts: 0},
        {word: "from", correct: false, attempts: 0},
        {word: "said", correct: false, attempts: 0},
        {word: "look", correct: false, attempts: 0},
        {word: "and", correct: false, attempts: 0},
        {word: "green", correct: false, attempts: 0},
        {word: "where", correct: false, attempts: 0},
        {word: "yellow", correct: false, attempts: 0}
    ],
    week29: [
        {word: "fly", correct: false, attempts: 0},
        {word: "bat", correct: false, attempts: 0},
        {word: "ring", correct: false, attempts: 0},
        {word: "wave", correct: false, attempts: 0},
        {word: "palm", correct: false, attempts: 0},
        {word: "scale", correct: false, attempts: 0},
        {word: "chest", correct: false, attempts: 0},
        {word: "ruler", correct: false, attempts: 0},
        {word: "right", correct: false, attempts: 0},
        {word: "well", correct: false, attempts: 0},
      ],
      week30: [
        {word: "rock", correct: false, attempts: 0},
        {word: "solid", correct: false, attempts: 0},
        {word: "water", correct: false, attempts: 0},
        {word: "below", correct: false, attempts: 0},
        {word: "back", correct: false, attempts: 0},
        {word: "front", correct: false, attempts: 0},
        {word: "small", correct: false, attempts: 0},
        {word: "tall", correct: false, attempts: 0},
        {word: "oval", correct: false, attempts: 0},
        {word: "America", correct: false, attempts: 0},
      ],
      week31: [
        {word: "gas", correct: false, attempts: 0},
        {word: "sad", correct: false, attempts: 0},
        {word: "summer", correct: false, attempts: 0},
        {word: "fall", correct: false, attempts: 0},
        {word: "winter", correct: false, attempts: 0},
        {word: "happy", correct: false, attempts: 0},
        {word: "long", correct: false, attempts: 0},
        {word: "short", correct: false, attempts: 0},
        {word: "small", correct: false, attempts: 0},
        {word: "africa", correct: false, attempts: 0},
      ], 

  }),
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  background-color: plum;
}
h1 {
    color: purple;  
  }
  
button {
    color: white;
    background-color: coral;
    font-size: 1em;
    padding: 10px;
    margin-top: 10px;
    margin-bottom: 10px;
    font-size: 1.5em;
    margin-right: 5px;
    margin-left: 5px;
  }
</style>
