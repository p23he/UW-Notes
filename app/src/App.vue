<template>
  <div id="app">
    
    <h2>Math in TeX notation</h2>
    <vue-mathjax :formula="formula"></vue-mathjax>
  </div>
</template>


<script lang="ts">

import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "./components/HelloWorld.vue";
import {VueMathjax} from 'vue-mathjax'

@Component({
  components: {
    HelloWorld,
    VueMathjax
  }
})
export default class App extends Vue {
  private formula: string = "Let $G=(V,E)$ be a graph.";
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
