<template>
  <div id="app">
    <Header @clickedHome="doShowHome()" @clickedSound="doShowSound()" @clickedDrag="doShowDrag()" @clickedBac="doShowBac()"/>
    <div class="container">
      <home v-if="showHome"/>
      <sound v-if="showSound"/>
      <DragNDrop v-if="showDrag"/>
      <Bac v-if="showBac"/>
    </div>
    <Footer/>
  </div>
</template>

<script>
import Home from "./components/Home.vue"
import Header from "./components/Header.vue"
import Bac from "./components/Bac.vue"
import DragNDrop from "./components/DragNDrop.vue"
import Sound from "./components/Sound.vue"
import Footer from "./components/Footer.vue"

export default {
  name: 'App',
  components: {
    Bac,
    Home,
    Header,
    DragNDrop,
    Sound,
    Footer
  },
  data(){
    return {
      showHome: true,
      showDrag: false,
      showSound: false,
      showBac: false,
    }
  },
  methods: {
    doShowHome(){
      this.showHome = true;
      this.showDrag = false;
      this.showSound = false;
      this.showBac = false;
    },
    doShowSound(){
      this.showHome = false;
      this.showDrag = false;
      this.showSound = true;
      this.showBac = false;
    },
    doShowDrag(){
      this.showHome = false;
      this.showDrag = true;
      this.showSound = false;
      this.showBac = false;
    },
    doShowBac(){
      this.showHome = false;
      this.showDrag = false;
      this.showSound = false;
      this.showBac = true;
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
