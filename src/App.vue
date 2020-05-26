<template>
  <div id="app">
    <CalculatorComponent msg="Calculator"/>
  </div>
</template>

<script>
import CalculatorComponent from './components/CalculatorComponent.vue'
export default {
  name: 'App',
  components: {
    CalculatorComponent
  }
}
</script>

<style src="./assets/css/tailwind.css"></style>
