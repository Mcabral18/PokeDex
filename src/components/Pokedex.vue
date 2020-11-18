<template>
  <div class="container">
    <h1 class="title">{{ title }}</h1>
    <form @submit.enter.prevent="searchPokemon">
      <input type="text" v-model="searchValue" placeholder="Search Pokemon" />
    </form>
    <div class="row">
      <div class="card card-input" v-if="inputSearch">
        <img
          class="card-img"
          :src="imageUrl + pokemonsearch.id + '.png'"
          alt=""
        />
        <div class="card-body">
          <h6>{{ pokemonsearch.id }}</h6>
          <h5 class="card-title">{{ pokemonsearch.name }}</h5>
        </div>
      </div>
    </div>
    <hr />
    <div class="row">
      <img v-if="loading" :src="gif" />
      <div class="searchbar"></div>
      <div
        class="card"
        v-for="pokemon in pokemons"
        :key="pokemon.id"
      >
        <img
          class="card-img-top"
          :src="imageUrl + pokemon.id + '.png'"
          alt="Card image cap"
        />
        <div class="card-body">
          <h6>{{ pokemon.id }}</h6>
          <h5 class="card-title">{{ pokemon.name }}</h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      title: "PokeDex",
      imageUrl: "https://pokeres.bastionbot.org/images/pokemon/",
      apiUrl: "http://pokeapi.co/api/v2/pokemon/?offset=0&limit=950",
      gif:
        "https://media.tenor.com/images/8f7efbb54fada6c671ab4eadfbe9b355/tenor.gif",
      pokemons: [],
      loading: false,
      searchValue: "",
      pokemonsearch: {},
      inputSearch: false,
    };
  },
  //create a function on windows load
  created() {
    this.sendApiRequest();
  },
  methods: {
    async sendApiRequest() {
      this.loading = true;
      this.pokemons = [];
      console.log("hello");
      // eslint-disable-next-line no-unused-vars
      const response = await fetch(`${this.apiUrl}`)
        .then((data) => data.json())
        .then((data) => {
          data.results.forEach((pokemon) => {
            pokemon.id = pokemon.url
              .split("/")
              .filter(function(part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        });
      this.loading = false;
      console.log(this.pokemons);
    },
    searchPokemon() {
      // eslint-disable-next-line no-unused-vars
      axios
        .get(`http://pokeapi.co/api/v2/pokemon/${this.searchValue}`)
        .then(({ data }) => (this.pokemonsearch = data))
        .catch(() => {
          document.querySelector(
            ".card-title"
          ).innerHTML = `<h6>"Misspelled Pokemon"</h6>`;
        });
      this.inputSearch = true;
    },
  },
};
//for some reason we need to display more functions like in the future we create a search pokemon
// created method to create multiple functions/methods
// created() {
//   this.sendApiRequest();
//    this.otherfunction();
// },
//display the multiple fuctions we create
// methods: {
//   sendApiRequest(){},
//   otherfunction (){}
// }
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Muli&display=swap");
@import url("https://fonts.googleapis.com/css?family=Lato:300,400&display=swap");

* {
  box-sizing: border-box;
}

body {
  font-family: "Muli", "Lato", sans-serif;
}
#app {
  background: linear-gradient(
    to bottom,
    rgba(246, 248, 249, 1) 0%,
    rgba(229, 235, 238, 1) 50%,
    rgba(215, 222, 227, 1) 51%,
    rgba(245, 247, 249, 1) 100%
  );
  margin-top: 0 !important;
}

.title {
  padding: 2rem;
  font-weight: 600;
  color: gray;
}

.row {
  justify-content: center;
  margin: 0 !important;
}

.card {
  margin: 1rem;
  box-shadow: 2px 2px 1px 1px grey;
}

.card-img-top {
  padding: 1rem;
  max-width: 300px;
}

.card-title {
  text-transform: uppercase;
}

.card-img {
  padding: 1rem;
  max-width: 300px;
}
.card-input {
  box-shadow: 5px 5px 13px 5px rgba(0, 0, 0, 0.35);
}
</style>
