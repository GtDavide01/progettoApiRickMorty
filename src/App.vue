<script>
import axios from "axios";
export default {
  data() {
    return {
      listCharacters: [],
      currentPage: 1,
      totalPages: 0,
      inputUser: "",
      selectedStatus: '',
      selectedSpecies: ''
    };
  },
  methods: {
    getUserCharacter() {
    let url = `https://rickandmortyapi.com/api/character/?name=${this.inputUser}`;
    if (this.selectedStatus) {
      url += `&status=${this.selectedStatus}`;
    }
    if (this.selectedSpecies) { // Aggiungi la selezione della specie all'URL solo se è stata selezionata
      url += `&species=${this.selectedSpecies}`;
    }
    axios.get(url).then(resp => {
      this.listCharacters = resp.data.results;
      this.totalPages = resp.data.info.pages;
    });
  },
  getListCharacters() {
    let url = `https://rickandmortyapi.com/api/character?name=${this.inputUser}&page=${this.currentPage}`;
    if (this.selectedStatus) {
      url += `&status=${this.selectedStatus}`;
    }
    if (this.selectedSpecies) { // Aggiungi la selezione della specie all'URL solo se è stata selezionata
      url += `&species=${this.selectedSpecies}`;
    }
    axios.get(url).then(resp => {
      this.listCharacters = resp.data.results;
      this.totalPages = resp.data.info.pages;
    });
  },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.getListCharacters();
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.getListCharacters();
      }
    }
  },
  created() {
    this.getListCharacters();
  }
};
</script>
<template>
  <header>
    <h1>Rick and Morty Api</h1>
    <div class="input">
      <input type="text" v-model="inputUser" @keyup.enter="getUserCharacter">
      <button @click="getUserCharacter">Cerca</button>
      <select v-model="selectedStatus">
        <option value="">Stato</option>
        <option value="alive">Vivo</option>
        <option value="dead">Morto</option>
      </select>
      <select v-model="selectedSpecies">
        <option value="">Specie</option>
        <option value="human">Umano</option>
        <option value="humanoid">Umanoide</option>
        <option value="alien">Alieno</option>
        <option value="animal">Animale</option>
        <option value="mythological creature">Creatura mitologica</option>
        <option value="cronenberg">Cronenberg</option>
        <option value="disease">Virus</option>
        <option value="robot">Robot</option>
      </select>
  </div>
  <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li  class="page-item me-3">
          <button class="page-link" @click="prevPage">Previous</button>
        </li>
        <li class="page-item">
          <button class="page-link" @click="nextPage">Next</button>
        </li>
      </ul>
    </nav>
  </header>
  <main>
    <div class="card" v-for="caracter in listCharacters">
      <div class="image">
        <img :src="caracter.image" alt="" />
      </div>
      <div class="info">
        <p>Nome: {{ caracter.name }}</p>
        <p>Sesso: {{ caracter.gender }}</p>
        <p>Stato: {{ caracter.status }}</p>
        <p>Specie: {{ caracter.species }}</p>
      </div>
    </div>
  </main>
  <footer>
   <h1>Creato da Davide Fiorini!</h1>
  </footer>
</template>


<!-- CSS/SCSS -->
<style lang="scss">
    *{
        padding: 0;
        margin: 0;
        box-sizing: border-box;
        font-family: 'Creepster', cursive;
    }
    footer{
        display: flex;
        justify-content: center;
        padding: 2rem;
    }
    header{
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px;
        h1{
            font-size: 3rem;
        } 
        .input{
          margin-bottom: 20px;
        }
          input{
            padding: 5px;
            margin-right: 15px;
            border-radius: 0.5rem;
            border: 1px solid lightgreen;
            background-color: lightblue;
          }
          button{
            padding: 5px;
            border-radius: 0.5rem;
            border: 1px solid lightgreen;
            background-color: lightblue;
          }
          select {
            padding: 7px;
            margin-left : 15px;
            border-radius: 0.5rem;
            border: 1px solid lightgreen;
            background-color: lightblue;
          }
    }
    main{
        flex-wrap: wrap;
        padding: 0 200px;
        display: flex ;
        gap : 1rem; 
        justify-content: center;
        .card{
            border-radius: 3rem; 
        min-width: 300px;
        background-color: lightblue;
        .image img{
            border-top-left-radius: 3rem;
            border-top-right-radius: 3rem;
        }
        .info{
            display: flex; 
            align-items: center;
            flex-direction: column;
            p{
                font-size: 20px;
            }
        }
    }
   
    }
   
    
</style>