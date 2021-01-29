<template>
  <div class="dot-dice-holder">
    <img :src="'./assets/images/dice-'+(number===0?'blank':number)+'.png'" :alt="'Dice with a number of '+number">
  </div>
</template>

<script>
export default {
  props: ['number']
}
</script>

<style>
div.dot-dice-holder{
  margin:0 10px;
  display:inline-block;
}
</style>