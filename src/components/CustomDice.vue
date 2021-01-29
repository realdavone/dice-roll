<template>
  <div class="custom-dice-holder">
    <span class="custom-dice-text">
      {{lines[number-1]}}
    </span>
    <img class="custom-dice-image" src="/assets/images/dice-blank.png" :alt="'Dice with a text of '+lines[number-1]">
  </div>
</template>

<script>
export default {
  props: ['number', 'index', 'lines']
}
</script>

<style>
div.custom-dice-holder{
  position:relative;
  width:100px;
  height:100px;
  padding:5px;
  display:inline-flex;
  justify-content:center;
  align-items:center;
  text-align:center;
  margin:0 10px;
}
img.custom-dice-image{
  position:absolute;
  top:0;
  left:0;
  z-index:1;
}
span.custom-dice-text{
  position:relative;
  z-index:2;
  color:#ffffff;
  font-size:0.8rem;
}
</style>