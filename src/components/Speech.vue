<template>
  <div>
  <h1 v-if="feedback.show" :style="feedback.color">{{feedback.string}}</h1>
  <div style="display: flex; justify-content: center;">
    <table style="display: inline-block;">
      <thead>
        <th>Vocabulary</th>
        <th>Counter</th>
        <th>Answer</th>
        <th>Help</th>
      </thead>
      <tr v-for="(word, index) in remainingWords" :key="index">
        <td>{{word.word}}</td>
        <td>{{word.attempts}}</td>
        <td><button @click="record(index)" >Click Here</button></td>
        <td><button @click="helpMe(index)">Click Here</button></td>
      </tr>
    </table>
     <table style="display: inline-block;">
      <thead>
        <th>Vocabulary</th>
        <th>Counter</th>
      </thead>
      <tr v-for="(word, index) in correctWords" :key="index">
        <td>{{word.word}}</td>
        <td>{{word.attempts}}</td>
      </tr>
    </table>
  </div>
  </div>
</template>

<script>
export default {
  name: 'Speech',
  props: ['words'],
    data: () => ({
      answer: "",
      question:"",
      feedback: {
        color: "color: purple;",
        string: "Please click a button to start",
        show: true
      },
      msg: null,
      recognition: null
  }),
  methods: {
    record(index){
      const vm = this;
      console.log("id:" + index);
      console.log(this.remainingWords[index].word);     
      this.recognition.onresult = function(event){
        console.log(event.results[0][0].transcript);
        vm.answer = event.results[0][0].transcript;
        if(vm.remainingWords[index].word.toLowerCase() == vm.answer.toLowerCase()){
          vm.remainingWords[index].attempts += 1;
          vm.remainingWords[index].correct = true;
          vm.giveFeedback("Fantastic Job Lilly!", "color: green;");    
        }else{
          vm.remainingWords[index].attempts += 1;
          vm.giveFeedback("Good Try, " + vm.answer + " is incorrect.", "color: red;");
        }
      }
      this.recognition.start(       
          vm.recording()
      );
    },
    helpMe(index){
     
      this.msg.text = this.remainingWords[index].word
      speechSynthesis.speak(this.msg);
    },
    recording(){
      this.feedback.color = "color: red;";
      this.feedback.string = "Recording...";
    },
    giveFeedback(string, color){
      this.feedback.color = color;
      this.feedback.string = string
      this.feedback.show = true;
      this.msg.text = string;
      speechSynthesis.speak(this.msg);
    }
  },
  computed: {
    remainingWords(){
      return this.words.filter(word => word.correct != true);
    },
    correctWords(){
      return this.words.filter(word => word.correct == true);
    },
  },
  beforeMount(){
    // Text to Speech
      this.msg = new SpeechSynthesisUtterance();
      var voices = window.speechSynthesis.getVoices();
      this.msg.voice = voices[1];
      this.msg.volume = 1;
      this.msg.rate = .8;
      this.msg.text = "";
      this.msg.lang = 'en-GB';
    
    // Speech to Text
      this.recognition = new window.webkitSpeechRecognition();
      this.recognition.lang = "en-GB";
  }
}
</script>


<style scoped>
  table {
    margin-right: 25px;
    border-collapse: collapse;
    border: black 4px solid;
    font-size: 1.5em;
  }
  th {
    border: 1px black solid;
    padding: 5px;
  }
  td {
    border: 1px black solid;
  }
  h1 {
    color: plum;  
  }
  button {
    color: white;
    background-color: purple;
    font-size: 1em;
    padding: 10px;
    margin-top: 10px;
    margin-bottom: 10px;
  }
  td::first-letter {
    text-transform: uppercase;
  }
</style>
