<template>
  <div id="app">
    <header class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">Characters</span>
        </h1>
        <button class="button is-success is-rounded" v-on:click="takeData">Show my People</button>
      </div>
    </header>
    <div class="container">
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
        <Character v-for="character in characters" v-bind:key="character.id" v-bind:character="character"/>
      </div>
    </div>  
  </div>
</template>

<script>
import axios from "axios";

import Character from "./components/Character.vue"

export default {
  name: 'App',
  components: {
    Character
  },
  data() {
    return {
      characters: []
    }
  },
  created(){
    this.takeData()
  },
  methods: {
    takeData() {
      let it = this;
      axios.get("https://rickandmortyapi.com/api/character")
        .then((res) => {
          console.log(res.data);
          it.characters = res.data.results;
          })
        .catch((err) => {
          console.log(err)
        })
    }
  }
}
</script>

<style>
</style>
