<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <vue-mathjax :formula="formula"></vue-mathjax>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { VueMathjax } from 'vue-mathjax';

@Component({
  components: {
    'vue-mathjax': VueMathjax
  }
})
export default class App extends Vue {
  private formula: string = "$$x = {-b \\pm \\sqrt{b^2-4ac} \\over 2a}.$$";
}
</script>

<style lang="less">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
