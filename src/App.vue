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
        @showModal="showModal"
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
    <div class="modal"
      :class="{ 'is-active': modal }"
      v-if="modal">
      <div class="modal-background" @click="modal=false"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">{{ details.species }}</p>
        </header>
        <div class="modal-card-body">
          <strong>Gender:</strong>
          <p>{{ details.gender }}</p>        
          <strong>Status:</strong>
          <p>{{ details.status }}</p>
          <strong>Specie:</strong>
          <p>{{ details.species }}</p>
          <strong>type:</strong>
          <p>{{ details.type }}</p>
        </div>
        <footer class="modal-card-foot">
          <button class="button" @click="modal=false">Close</button>
        </footer>

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
      characters: [],
      page: 1,
      pages: 1,
      search: '',
      modal: false,
      details: {}
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
    },
    showModal(id) {
      this.modal = true;
      this.getDetails(id);
    },
    async getDetails(id){
       let respDet = await axios.get(`https://rickandmortyapi.com/api/character/${id}/`)
       this.details = respDet.data;
       console.log(this.details);
    }
  }
}
</script>

<style>
</style>
