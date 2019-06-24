<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <button data-html2canvas-ignore v-on:click="takeScreenshot">Screenshot</button>
    <transition name="fade">
      <div id="overlay" v-if="showOverlay"></div>
    </transition> 
  </div>
</template>

<script>
import jsPDF from 'jspdf'
import html2canvas from 'html2canvas'
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data() {
    return {
      showOverlay: false
    }
  },
  methods: {
    takeScreenshot: async function() {
      let self = this;
      this.showOverlay = true;
      setTimeout(() => {
        self.showOverlay = false;
      }, 500);
      let canvas = await html2canvas(document.body);
      let doc = jsPDF('l', 'mm', 'a4');
      let width = doc.internal.pageSize.getWidth();
      let height = doc.internal.pageSize.getHeight();
      doc.addImage(canvas, 'PNG', 0, 0, width, height);
      doc.save('screenshot.pdf');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: white;
  z-index: 2;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .1s;
}
</style>
