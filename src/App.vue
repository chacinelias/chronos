<template>
  <div id="app" class="bg-dark">
    <NavBar :activeCollection='activeCollection' @chooseCollection='chooseCollection($event)' />
    <div id='container' class='container jumbotron mt-5 p-2'>
      <keep-alive>
        <component :is='activeCollection'></component>
      </keep-alive>
    </div>
    <Footer />
  </div>
</template>

<script>
import WCollection from './components/WCollection.vue';
import TCollection from './components/TCollection.vue';
import NavBar from './components/Nav.vue';
import Footer from './components/Footer';

export default {
  name: 'app',
  components: {
    WCollection,
    TCollection,
    NavBar,
    Footer
  },

  data() {
    return {
      activeCollection: WCollection
    };
  },

  methods: {
    chooseCollection(c) {
      this.activeCollection = !c ? WCollection : TCollection;
    }
  }
}
</script>

<style>

html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}

#app {
  height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}

.large-font {
  font-size: 72px;
}

.medium-font {
  font-size: 36px;
}

.small-font {
  font-size: 20px;
}

.white-font {
  color: white;
}

.yellow {
  background-color: #fbb901;
}

.orange {
  background-color: #774898;
}

.running {
  background-color: #00a8b5;
}

.paused {
  background-color: #e62a76;
}

.end {
  background-color: #e62a76;
}

#close {
  font-size: 68px;
}
</style>
