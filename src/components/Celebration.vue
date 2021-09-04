<template>
  <div>
    <img v-bind:src="'/assets/logo.png'"/>
  </div>
</template>

<script>
export default {
  setup () {
    var audio = new Audio("./assets/size_matters.mp3")
    audio.play();
  }
}
</script>

<style lang="css" scoped>

</style>