<template>
            <template  v-if="!this.endGame" >
                <button v-if="!this.hard" class="send" @click="hardTime()">mode difficile</button>
                <button v-else  class="send" @click="hardTime()" >mode simple</button> 
            </template>
    <div>
       <scoreBoard  :computerScore = this.computerScore :userScore = this.userScore />

      <template v-if="this.question &&  !this.endGame">
      <h1 v-html="question">
      </h1>
    
      <template v-for="(answer, index) in this.answers" :key="index" >
         <input  
          :disabled="this.userAnswerSubmitted && this.goodAnswer"
          type="radio"
          name="options"
          :value="answer"
          v-model="userAnswer">

          <label v-html="answer"></label><br>
      </template>

      </template>

      <button :disabled="this.goodAnswer" v-if="!this.endGame" @click="submit(), match()" class="send" type="button">Send</button>
      <button v-else class="send" type="button" @click="newGame(); "> New Game </button>

    </div>
    

    
    <section class="result">
      <template v-if="userAnswerSubmitted">
        <h4 v-if="this.goodAnswer" v-html= " '&#9989; nice!!! The correct is '+ this.correctAnswer+'.' "> </h4>
        <h4 v-else-if="!this.goodAnswer"  v-html= " 'I`m sorry, you picked the wrong answer. The correct is  '+ this.correctAnswer + ' .  <br> try again '+ this.tryAgain +'.'"></h4>
      </template>
      <button v-if="!this.endgame"
              :disabled="!this.goodAnswer"
              @click="newQuestion()" 
              class="send"
              type="button">Next question</button>
    </section>
  
</template>

<script>
import scoreBoard from './components/score.vue'
export default {
  name: 'App',
  components:{
    scoreBoard,
  },
 
  created() {
    this.hard = false
    this.newQuestion();
  },

  //      limite de points
  //      version difficile

  computed: {
    answers(){
     var answer = JSON.parse( JSON.stringify(this.incorrectAnswer) )
         answer.splice(Math.round(Math.random() * answer.length), 0, this.correctAnswer);
     return answer
    }
 
  },

  methods:{
      submit(){
        if(!this.userAnswer){
           alert('fait un choix')
        }
        else {
          this.userAnswerSubmitted = true;
          if(this.userAnswer == this.correctAnswer){
            this.userScore ++
            this.goodAnswer = true
          }
          else{
             this.goodAnswer = false
             this.computerScore ++
             this.tryAgain--
             if(this.tryAgain == 0){
               alert("game over")
               this.endGame = true;
             }
          }
          
        }
      },
      newGame(){
        window.location.reload();     
         },

      match(){
        if(this.userScore >= this.limit){
          alert('you win !!!');
          this.endGame = true

        }
      }
      ,
      hardTime(){
      this.hard =  !this.hard
      this.newQuestion();
      this.computerScore = 0;
      this.userScore = 0;
      if(this.hard){
      this.limit = 10;
      this.tryAgain = 1
      }
      },

      newQuestion(){
         this.userAnswerSubmitted = false,
         this.userAnswer = undefined;
         this.goodAnswer = false;



         const api = ["https://opentdb.com/api.php?amount=10&category=11&difficulty=easy&type=multiple", "https://opentdb.com/api.php?amount=10"];
         function choice(statut){
          let result;
            if(statut){
             result = api[1];
            console.log(statut)
            return result
         }
         else{
           result = api[0]
           console.log(statut)
           return result
         }
         }
         
            
        
     this.axios.get(choice(this.hard)).then((response) => {
      this.question = response.data.results[0].question
      this.correctAnswer = response.data.results[0].correct_answer
      this.incorrectAnswer = response.data.results[0].incorrect_answers
    })
      }

  },
  
  data(){
    return{
      question: undefined,
      correctAnswer: undefined,
      incorrectAnswer: undefined,
      userAnswer: undefined,
      userAnswerSubmitted: false,
      computerScore: 0,
      userScore: 0,
      tryAgain: 3,
      endGame: false,
      limit: 5,
      hard: false,
      goodAnswer: false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

}

h1 {
  margin-top: 40px;
}

input[type='radio']{
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}


</style>
