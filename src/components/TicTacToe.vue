<script setup lang="ts">
import { ref } from 'vue'

//Sets up the 3x3 board, initially as empty 
const tics = ref({
    a1: '', a2: '', a3: '',
    b1: '', b2: '', b3: '',
    c1: '', c2: '', c3: '',
});

//Sets the first player to either 'O' or 'X'
const player = ref((Math.random() >= 0.5) ? "One" : "Two");

const gameOver = ref('');

//Resets the board and first player, restarting the game
const handleReset = ()=>{
    tics.value = {
    a1: '', a2: '', a3: '',
    b1: '', b2: '', b3: '',
    c1: '', c2: '', c3: '',
};
player.value = (Math.random() >= 0.5) ? "One" : "Two";
gameOver.value = '';
}

const checkStatus = ()=>{
    const checkAllEmpties = Object.entries(tics.value).flatMap((entry) => entry[1]).filter(entry => entry === '');
    //Ends game with tie if all boxes are filled
    if(checkAllEmpties.length === 0) gameOver.value = "Tied" ;
 //
    if(tics.value.a1 && tics.value.a1 === tics.value.a2 && tics.value.a1 === tics.value.a3) gameOver.value = player.value;
    if(tics.value.b1 && tics.value.b1 === tics.value.b2 && tics.value.b1 === tics.value.b3) gameOver.value = player.value;
    if(tics.value.c1 && tics.value.c1 === tics.value.c2 && tics.value.c1 === tics.value.c3) gameOver.value = player.value;
    if(tics.value.a1 && tics.value.a1 === tics.value.b1 && tics.value.a1 === tics.value.c1) gameOver.value = player.value;
    if(tics.value.a2 && tics.value.a2 === tics.value.b2 && tics.value.a2 === tics.value.c2) gameOver.value = player.value;
    if(tics.value.a3 && tics.value.a3 === tics.value.b3 && tics.value.a3 === tics.value.c3) gameOver.value = player.value;
    if(tics.value.a1 && tics.value.a1 === tics.value.b2 && tics.value.a1 === tics.value.c3) gameOver.value = player.value;
    if(tics.value.c1 && tics.value.c1 === tics.value.b2 && tics.value.c1 === tics.value.a3) gameOver.value = player.value;
}

const handleClick = (key: 'a1' | 'a2' | 'a3' | 'b1' | 'b2' | 'b3' | 'c1' |'c2' |'c3') =>{
    if(gameOver.value) return
    if(tics.value[key] !== '') return
    tics.value[key] = player.value;
    checkStatus()
    if(gameOver.value) return
    player.value = player.value === 'One'? "Two" : 'One';

}

const handleMessage = ()=> {
    if(gameOver.value === 'Tied') return "Tie!"
    if(gameOver.value === 'One' || gameOver.value === 'Two') return `Player ${player.value === 'One'? 'X' : 'O'} wins!`
    return `It is player ${player.value === 'One'? 'X' : 'O'}'s turn`
}


</script>

<template>
   <h2>{{handleMessage()}}</h2>
   <button v-if="gameOver == ''" @click="handleReset()">Restart</button>
   <button v-if="gameOver !== ''" @click="handleReset()">Play Again</button>

   <div class="tile-container">
        <div v-for="value,key in tics" @click="handleClick(key)" class="tiles">
            {{value === 'One'? 'X': value === 'Two'? 'O': ''}}
        </div>
   </div>

</template>

<style>
   :root {
    font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;
  
    color-scheme: light dark;
    color: rgba(255, 255, 255, 0.87);
    background-color: #242424;
  
    font-synthesis: none;
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    -webkit-text-size-adjust: 100%;
  }
  
  a {
    font-weight: 500;
    color: #646cff;
    text-decoration: inherit;
  }
  a:hover {
    color: #535bf2;
  }
  
  body {
    margin: 0;
    display: flex;
    place-items: center;
    min-width: 320px;
    min-height: 100vh;
  }
  
  h1 {
    font-size: 3.2em;
    line-height: 1.1;
  }
  
  button {
    border-radius: 8px;
    border: 1px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    background-color: #ffffff;
    cursor: pointer;
    transition: border-color 0.25s;
  }
  button:hover {
    border-color: #646cff;
  }
  button:focus,
  button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
  }
  
  .card {
    padding: 2em;
  }
  
  #app {
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;
    text-align: center;
  }
  
  .tile-container{
    margin: auto;
    display: grid;
    grid-template-columns: auto auto auto;
    padding: 2em;
    margin: 2em;
  }
  .tiles{
    width: 60px;
    height: 30px;
    background-color: #fff;
    color: #0000ff;
    padding: 1em;
    border: 2px solid #242424;
  }
</style>