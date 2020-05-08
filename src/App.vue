<template>
  <div id="app">
    <div class="wrapper">
      <div class="player-0-panel" v-bind:class="{active:player1.turn}">
        <div class="player-name" id="name-0">Player 1</div>
        <p v-if="player1.score >= winningScore">WINNER!!!</p>
        <div class="player-score" id="score-0">{{player1.score}}</div>
        <div class="player-current-box">
          <div class="player-current-label">Current</div>
          <div class="player-current-score" id="current-0">{{player1.current}}</div>
        </div>
      </div>

      <div class="player-1-panel" v-bind:class="{active:player2.turn}">
        <div class="player-name" id="name-1">Player 2</div>
        <p v-if="player2.score >= winningScore">WINNER!!!</p>
        <div class="player-score" id="score-1">{{player2.score}}</div>
        <div class="player-current-box">
          <div class="player-current-label">Current</div>
          <div class="player-current-score" id="current-1">{{player2.current}}</div>
        </div>
      </div>

      <button class="btn-new" v-on:click="init">
        <i class="ion-ios-plus-outline"></i>New Game
      </button>
      <button class="btn-roll" v-on:click="roll">
        <i class="ion-ios-loop"></i>Roll Dice
      </button>
      <button class="btn-hold" v-on:click="hold">
        <i class="ion-ios-download-outline"></i>Hold
      </button>
      <img v-show="isHidden" v-bind:src="diceImage" alt="Dice" class="dice" id="dice-1" />
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
      return{
       isActive:true,
       isHidden:false,
       gamePlaying : true,
       winningScore : 20,
       dice : 0,
       diceImage :'',
       roundScore:0,
       
       player1:{
           score:0,
           current:0,
           turn:true
       },
       player2:{
           score:0,
           current:0,
           turn:false
       },
      }
  },
  methods: {
     init:function(){
       this.gamePlaying = true,
       this.player1.score = 0,
       this.player1.current = 0,
       this.player2.score = 0,
       this.player2.current = 0
     },

     roll: function(){
         this.isHidden = true;
       if(this.gamePlaying){
         if(this.player1.score >= this.winningScore || this.player2.score >= this.winningScore){
             this.gamePlaying = false;
         }else{
             this.dice = Math.floor(Math.random() * 6) + 1;  //random number
             this.diceImage = 'dice-' + this.dice + '.png';

             if(this.player1.turn){
                 if(this.dice === 1){     
                     this.player1.current = 0;
                     this.player1.turn = false;
                     this.player2.turn = true;
                     return;
                 }
                     this.player1.current += this.dice;   //Updating the round score for  player1  If the rolled number was NOT a 1
             }
             if(this.player2.turn){
                 if(this.dice === 1){
                     this.player2.current = 0;
                     this.player2.turn = false;
                     this.player1.turn = true;
                     return;
                 }
                     this.player2.current += this.dice;  //Updating the round score for  player2 If the rolled number was NOT a 1
             }
         }
       }
     },
     hold: function(){
     if(this.gamePlaying){
         if(this.player1.turn){
             this.player1.turn = false;
             this.player2.turn = true;
             this.player1.score += this.player1.current;   //adding player1's currentscore to his globalscore
             this.player1.current = 0;
             return;
         }
         if(this.player2.turn){
             this.player2.turn = false;
             this.player1.turn = true;
             this.player2.score += this.player2.current;   //adding player2's currentscore to his globalscore
             this.player2.current = 0;
             return;
         }
      
     }
     
     },
  }
}
</script>

<style>
body {
  padding: 0px;
  margin: 0px;
  box-sizing: border-box;
}
.wrapper {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid black;
  width: 50%;
  height: 500px;
  display: table;
  float: left;
  text-align: center;
  background-color: #141e23d9;
}
p{
    font-size:23px;
    color:red;
}
.player-0-panel {
  display: table-cell;
  width: 43%;
  vertical-align: top;
  float: left;
  color: white;
  height: 500px;
}

.player-1-panel {
  display: table-cell;
  width: 43%;
  vertical-align: top;
  float: right;
  color: white;
  height: 500px;
}

.player-name {
  font-size: 30px;
  margin: 45px 0px;
}

.player-score {
  font-size: 30px;
  padding: 60px 0px;
}
.player-current-box {
  width: 100px;
  height: 80px;
  background-color: #00bcd4;
  margin: 65px auto;
  font-size: 20px;
}

.player-current-label {
  margin-bottom: 15px;
}

#dice-1 {
  position: absolute;
  height: 80px;
  top: 32%;
  left: 50%;
  transform: translate(-50%);
}

button {
  position: absolute;
  transform: translate(-50%);
  cursor: pointer;
}

.btn-new {
  margin-top: 25px;
  left: 50%;
  background-color: #00bcd4;
  padding: 10px;
  outline: none;
  border: none;
  color: white;
  font-size: 20px;
}

.btn-roll {
  top: 66%;
  left: 50%;
  background-color: #00bcd4;
  padding: 5px;
  outline: none;
  border: none;
  color: white;
  font-size: 20px;
}

.btn-hold {
  top: 82%;
  left: 50%;
  background-color: #00bcd4;
  padding: 5px;
  outline: none;
  border: none;
  color: white;
  font-size: 20px;
}
button i {
  margin-right: 3px;
}
.active {
  font-weight: 600;
  background-color: #607d8b;
}
.chooseNum {
  position: absolute;
  top: 85%;
  left: 45%;
  width: 100px;
  height: 25px;
  text-align: center;
  border: 1px solid #00bcd4;
}
</style>
