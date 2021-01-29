<template>
  <main>
    <div class="outter-wrapper">
      <header>
        <input type="radio" id="dots" value="dots" v-model="diceType">
        <label :class="diceType==='dots'?'active':''" for="dots">Dots dice</label>
        <input type="radio" id="custom" value="custom" v-model="diceType">
        <label :class="diceType==='custom'?'active':''" for="custom">Custom dice</label>
      </header>
      <div class="dice-holder-outter" v-if="diceType==='dots'">
        <DotsDice v-for="(dice, index) in dices" :key="index" :number="dice" />
      </div>
      <div v-else>
        <div class="dice-holder-outter">
          <CustomDice v-for="(dice, index) in dices" :key="index" :number="dice" :lines="customDiceLines"/>
        </div>
        <br>
        <form class="custom-dice-form" @submit.prevent="saveLines()">
          <input
            class="custom-dice-text-input"
            v-for="(sideText, index) in numberOfSides" 
            required 
            :key="index"
            v-model="customDiceLines[index]"
            type="text"
            :placeholder="'Custom text for '+(index+1)+'-dot side'">
            <button class="custom-dice-button">Save texts</button>
        </form>
      </div>
    </div>
    <div class="dice-action-button-holder">
      <button
        class="dice-action-button"
        v-if="dices.length<5" 
        @click="addDice()"
        >Add dice</button>
      <button
        class="dice-action-button"
        :disabled="!savedLines&&diceType==='custom'"
        @click="rollTheDice()"
        >Roll the dice</button>
    </div>
    <div class="dice-action-button-holder">
      <button
        class="dice-action-button"
        v-if="dices.length>1" 
        @click="removeDice()"
        >Remove dice</button>
      <button 
        class="dice-action-button"
        v-if="dices.length>1&&diceType==='dots'" 
        @click="calculateSum()"
        >Calculate sum</button>
    </div>
    <p 
      v-if="sumOfDice!==null"
      >Sum of the dice is {{sumOfDice}}</p>
  </main>
</template>

<script>
import DotsDice from './components/DotsDice';
import CustomDice from './components/CustomDice';
import { ref, watch } from 'vue';

export default {
  components: { DotsDice, CustomDice },
  setup() {
    const diceType = ref('dots');
    const dices = ref([0]);
    const sumOfDice = ref(null);
    const customDiceLines = ref([]);
    const numberOfSides = ref(6);
    const savedLines = ref(false);

    changeTitle(diceType.value);

    function rollTheDice() {
      for(let i=0; i<dices.value.length;i++) {
        dices.value[i] = (Math.floor(Math.random() * Math.floor(numberOfSides.value))) + 1;
      }
    }

    function addDice() {
      dices.value.push(0);
    }

    function calculateSum() {
      sumOfDice.value = dices.value.reduce((accumulator, number) => accumulator + number);
    }

    function saveLines() {
      savedLines.value = true;
    }

    function removeDice() {
      dices.value.pop();
    }

    function changeTitle(type) {
      document.title=`${type.charAt(0).toUpperCase()+type.slice(1)} dice roll`;
    }

    watch(diceType, function() {
      dices.value = [0];
      sumOfDice.value = null;
      changeTitle(diceType.value);
    })

    return { diceType, dices, rollTheDice, addDice, sumOfDice, calculateSum, customDiceLines, numberOfSides, saveLines, savedLines, removeDice }
  }
}
</script>

<style>
*{
  box-sizing:border-box;
  font-family: 'Open Sans', sans-serif;
  padding:0;
  margin:0;
}
body{
  background:#323241;
  padding:0 5%;
  min-height:100vh;
  color:#ffffff;
}
button{
  cursor:pointer;
  outline:none;
}
main{
  text-align:center;
}
header{
  padding:25px 0;
}
input[type=radio]{
  display:none;
}
label[for=dots],label[for=custom],button.dice-action-button{
  width:120px;
  padding:5px;
  margin:0 5px;
  display:inline-block;
  cursor:pointer;
  border-radius:5px;
  transition:0.3s ease;
  border:2px solid#b10000;
}
label[for=dots]:hover,label[for=custom]:hover,button.dice-action-button:hover{
  background:#c90000;
}
label[for=dots].active,label[for=custom].active{
  background:#b10000;
}
form.custom-dice-form{
  max-width:300px;
  display:inline-flex;
  flex-direction:column;
  margin:20px 0;
}
input.custom-dice-text-input{
  padding:5px;
  display:block;
  margin-bottom:5px;
  text-align:center;
  border-radius:5px;
  border:none;
}
div.dice-holder-outter{
  min-height:150px;
  display:inline-flex;
  align-items:center;
  flex-wrap:wrap;
  justify-content:center;
}
button.custom-dice-button{
  background:#d38c08;
  outline:none;
  border:none;
  color:#ffffff;
  padding:10px;
  border-radius:3px;
}
div.dice-action-button-holder{
  margin:10px 0;
}
button.custom-dice-button:hover{
  background:#e79a09;
}
button.dice-action-button{
  background:#b10000;
  color:#ffffff;
}
button:disabled{
  cursor:not-allowed;
  background:#ffffff28!important;
}
</style>
