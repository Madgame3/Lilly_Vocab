<template>
  <div style="display: flex; flex-direction: column; justify-content: center;">
    <h1>{{remainingWords.length}} Words Remaining</h1>
    <!-- cat images -->
    <img v-if="catImage !=null" :src="catImage.url" :width="catImage.width" :height="catImage.height">
    <button @click="playWord">Play Word</button>
    <input type="text" ref="input" v-model="answer" />
    <button @click="checkAnswer">Check My Answer</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Spelling',
  props: ['words'],
    data: () => ({
      index: null,
      catImage: null,
      answer: "",
      question: null,
      feedback: {
        color: "color: purple;",
        string: "Please click a button to start",
        show: true
      },
      msg: null,
      recognition: null
  }),
  methods: {
    randomWord(){
      this.index = Math.floor(Math.random() * this.remainingWords.length);
      this.question = this.remainingWords[this.index].word
      console.log("index: " + this.index + ", word: " + this.question);
    },
    playWord(){
      if(this.question == null && this.index == null){
        this.randomWord();
      }
      this.msg.text = this.question;
      speechSynthesis.speak(this.msg);
      this.$refs.input.focus();
    },
    async getCatImage(){
      try {
        const response = await axios.get('https://api.thecatapi.com/v1/images/search?mime_types=gif')
        this.catImage = response.data[0]
      } catch (error) {
        console.log(error);
      }
    },
    checkAnswer(){
      if(this.answer.toLowerCase() == this.question.toLowerCase()){
        this.getCatImage();
        this.giveFeedback("Fantastic!", true);
        this.remainingWords[this.index].correct = true;
        this.answer = ""
        this.question = null;
        this.index = null;
        if(this.remainingWords.length > 0){
          this.randomWord();
        }
      }else{
        this.giveFeedback("Good try, but no!", false);
        this.answer = "";
      }
    },
    giveFeedback(string, correct){
      this.msg.text = string;
      this.msg.rate = .8
      speechSynthesis.speak(this.msg); 
      console.log(string);
      if(!correct){
        this.msg.text = this.questionArray; 
        this.msg.rate = .5
        speechSynthesis.speak(this.msg); 
      }
    }
  },
  computed: {
    remainingWords(){
      return this.words.filter(word => word.correct != true);
    },
    correctWords(){
      return this.words.filter(word => word.correct == true);
    },
    questionArray(){
      return this.question.split('').join(',');
    }
  },
  beforeMount(){
    // Text to Speech
      this.msg = new SpeechSynthesisUtterance();
      var voices = window.speechSynthesis.getVoices();
      this.msg.voice = voices[1];
      this.msg.volume = 1;
      this.msg.rate = 1;
      this.msg.text = "";
      this.msg.lang = 'en-GB';
  },
  watch: {
    catImage(){
      var vm  = this;
      setTimeout(function(){
        vm.catImage = null
      }, 8000);
    },
    words(){
      console.log("Words changed");
      this.randomWord();
    }
  }
}
</script>


<style scoped>
h1 {
  color: purple;
}
button {
  color: white;
  background-color: purple;
  font-size: 1em;
  padding: 10px;
  margin-right: 10%;
  margin-left: 10%;
  margin-top: 15px;
  margin-bottom: 15px;
}
input {
  font-size: 5em;
  margin-right: 10%;
  margin-left: 10%;
  text-align: center;
}
img {
  margin: auto;
}
</style>
