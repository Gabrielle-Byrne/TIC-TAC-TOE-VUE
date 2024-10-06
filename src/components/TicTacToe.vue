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