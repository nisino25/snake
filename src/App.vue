<template>

  <button @click="rollDice()" v-if="!gameOver">ROLL</button>
  <button @click="reset()" v-else>Reset</button>
  <h1>Current Position: {{previousPosition}} --> {{currentPosition}}</h1>
  <h2>Dice Number: {{diceNum}}</h2>
  <span v-if="message !== ''">Message: </span> <span v-if="message !== ''"   :style="specialCollor">{{message}}</span>

  <h3>Move count: {{moveCount}}</h3>
  <h3>Ladder count: {{ladderCount}}</h3>
  <h3>Snake count: {{snakeCount}}</h3>

  <hr>

  <div>
    <div v-for="(Position, i) in specialPostion" :key="i" style="text-align:center">
      <strong v-if="!snakeCheck(Position.start,Position.end)">{{Position.start}} --> {{Position.end}}</strong>
    </div>

    <hr>

    <div v-for="(Position, i) in specialPostion" :key="i" style="text-align:center">
      <strong v-if="snakeCheck(Position.start,Position.end)">{{Position.start}} --> {{Position.end}}</strong>
    </div>
  </div>



</template>

<script>


export default {
  name: 'App',
  
  data(){
    return {

      user: {
        name: 'Nozomu',
        email: '',
        password: ''
      },

      currentPosition: 0,
      previousPosition: 0,
      moveCount: 0,
      diceNum: null,
      message: "",
      gameOver: false,
      ladderCount: 0,
      snakeCount: 0,
      specialCollor: null,
      multiplayer: false,
      

      specialPostion:[{start:2, end: 38 },{start: 7, end: 14 }, {start:16 , end:6 },{start:21,end:42},{start:36,end:44},{start:41,end:42},{start:46,end:25},{start:51,end:67},{start:62,end:19},{start:64,end:60},{start:74,end:53},{start:78,end:98},{start:87,end:24},{start:92,end:88},{start:95,end:75},{start:99,end:80}],
      


    }
  },
  methods:{
    rollDice(){
      this.message = ''
      this.previousPosition = this.currentPosition
      this.diceNum = 1 + Math.floor(Math.random() * (6));
      this.currentPosition = this.currentPosition + this.diceNum
      
      if(this.currentPosition === 100){
        this.currentPosition = 100;
        alert('You just won!')
        this.gameOver = true
      }else if(this.currentPosition > 100){
        this.currentPosition = 100 - (this.currentPosition - 100) ;
      }
      
      this.moveCount++

      this.specialCheck()

      


    },

    specialCheck(){
      let counter = 0;
      let flag = false;
      while(counter < 16 && !flag){
        if(this.currentPosition === this.specialPostion[counter].start){
          this.currentPosition = this.specialPostion[counter].end
          if(this.snakeCheck(this.specialPostion[counter].start,this.specialPostion[counter].end)){
            this.message = "Snake"
            this.specialCollor = 'color:red';
            this.snakeCount++
          }else{
            this.message = "Ladder"
            this.specialCollor = 'color:blue';
            this.ladderCount++
          }
          flag = true
          
        }
        counter++
        // console.log(counter)
      }
    },

    snakeCheck(num1,num2){
      if(num1 > num2){
        return true
      }else{
        return false
      }
    },

    reset(){
      this.currentPosition = 0;
      this.gameOver = false;
      this.moveCount = 0
      this.message = ''
      this.previousPosition = 0
      this.diceNum = null 

      this.snakeCount = 0
      this.ladderCount = 0
    }
  },

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
  touch-action: manipulation;
}
</style>
