<template>
  <div id="app">
    <header class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">Characters</span>
        </h1>
        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input v-model="search" type="text" class="input is-rounded" v-on:keyup.enter="searchData">
          </div>
          <div class="control">
            <button class="button is-success is-rounded" v-on:click="searchData">Search by Name</button>
          </div>
        </div>
      </div>
    </header>
    <div class="container">
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
        <Character 
        v-for="character in characters" 
        v-bind:key="character.id" 
        v-bind:character="character"/>
      </div>
      <nav class="pagination" role="navegation" aria-label="pagination">

        <a class="pagination-previous" v-on:click="changePage( page - 1 )">Before</a>  

        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>

        <a class="pagination-next" v-on:click="changePage( page + 1 )">Next</a>      

      </nav>
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
      characters: [],
      page: 1,
      pages: 1,
      search: ''
    }
  },
  created(){
    this.takeData()
  },
  methods: {
    takeData() {
      let params = {
        page : this.page,
        name : this.search
      };
      let it = this;
      axios.get("https://rickandmortyapi.com/api/character/", { params })
        .then((res) => {
          console.log(res.data);
          it.characters = res.data.results;
          it.pages = res.data.info.pages
          })
        .catch((err) => {
          console.log(err)
        })
    },
    changePage(receivedPag) {
      this.page = (receivedPag > 0 && receivedPag < this.pages )? receivedPag : this.page;
      this.takeData();
    },
    searchData() {
      this.page = 1;
      this.takeData()
    }
  }
}
</script>

<style>
</style>
