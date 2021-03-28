<template>
  <h1 class="title">Richard’s CodeWars Code Katas</h1>
  <div>
    <div class="input-group">
      <div class="input">
        <label for="search">Search by Title:</label>
        <input
          id="search"
          placeholder="Search..."
          @input="debounceSearch($event)"
        />
      </div>
      <div class="input">
        <label for="select">Select Rank:</label>
        <select name="Kata Rank" id="select" v-model.number="kataRank">
          <option value="">All Katas</option>
          <option value="1">1 Kyu</option>
          <option value="2">2 Kyu</option>
          <option value="3">3 Kyu</option>
          <option value="4">4 Kyu</option>
          <option value="5">5 Kyu</option>
          <option value="6">6 Kyu</option>
          <option value="7">7 Kyu</option>
          <option value="8">8 Kyu</option>
        </select>
      </div>
    </div>
  </div>
  <p class="count">Number of Challenges: {{ filteredKatas.length }}</p>
  <div class="card-container">
    <template v-if="!filteredKatas.length">
      No matching katas
    </template>
    <template v-else>
      <BaseCard
        v-for="(challenge, index) in filteredKatas"
        :key="'kata' + index"
      >
        <template v-slot:title>
          <h1>{{ challenge.title }}</h1>
        </template>
        <template v-slot:hidden>
          <h2>{{ challenge.kyu }} Kyu</h2>
          <a
            :href="challenge.code"
            v-text="solText"
            target="_blank"
            class="link"
          />
          <a
            :href="challenge.challenge"
            v-text="challText"
            target="_blank"
            class="link"
          />
        </template>
      </BaseCard>
    </template>
  </div>
</template>

<script>
import BaseCard from "./components/BaseCard.vue";
import { katas } from "./data.js";
let debounceTimeout;

export default {
  name: "App",
  components: {
    BaseCard,
  },
  data() {
    return {
      katas: katas,
      kataRank: "",
      searchTerm: "",
      solText: "Go to my solution",
      challText: "Try the Challenge",
    };
  },
  methods: {
    debounceSearch(event) {
      if (debounceTimeout) {
        clearTimeout(debounceTimeout);
      }
      debounceTimeout = setTimeout(() => {
        this.searchTerm = event.target.value;
      }, 500);
    },
  },
  computed: {
    filteredKatas() {
      let filteredKatas = this.katas;
      if (this.kataRank) {
        const katasFilteredByRank = filteredKatas.filter(
          (kata) => kata.kyu == this.kataRank
        );
        filteredKatas = katasFilteredByRank;
      }
      if (this.searchTerm) {
        const katasFilteredBySearch = filteredKatas.filter((kata) => {
          const searchTerm = this.searchTerm.toLowerCase();
          const title = kata.title.toLowerCase();
          return title.includes(searchTerm);
        });
        filteredKatas = katasFilteredBySearch;
      }
      return filteredKatas;
    },
  },
};
</script>

<style>
#app {
  font-family: "Ribeye", cursive;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.title {
  font-size: clamp(1.5rem, 2.5rem, 6rem);
  margin: 10px 10px 30px 10px;
}
.link {
  color: #fff;
  margin: 5px;
}
h2 {
  color: #fff;
  padding: 5px 10px;
  border: 2px solid #fff;
  border-radius: 100vh;
}
.card-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  margin: 50px 20px;
}
input,
select {
  font-family: "Ribeye", cursive;
  color: #2c3e50;
  margin: 30px 0px;
  padding: 15px 30px;
  outline-color: #000;
}
option {
  color: #2c3e50;
}
input {
  min-width: 50vw;
}
.count {
  font-size: 1.3rem;
}
.input-group {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
.input {
  position: relative;
  margin: 2px;
}
.input label {
  position: absolute;
  top: 0px;
  left: 10px;
}
</style>
