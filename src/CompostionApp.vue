<template>
  <h1 class="title">Richard’s CodeWars Code Katas</h1>
  <div>
    <input type="text" placeholder="Search..." v-model="searchTerm" />
    <select name="" id="" v-model="kataRank" @change="updateList">
      <option value="Select Rank">Select Rank</option>
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
  <p class="count">Number of Challenges: {{ CodeCount }}</p>
  <div class="card-container">
    <Card v-for="challenge in kataData" :key="challenge.id">
      <template v-slot:title>
        <h1>{{ challenge.title }}</h1>
      </template>
      <template v-slot:hidden>
        <h2>{{ challenge.kyu }} Kyu</h2>
        <a :href="challenge.code" target="_blank" class="link"
          >Go to my solution</a
        >
        <a :href="challenge.challenge" target="_blank" class="link"
          >Try the Challenge</a
        >
      </template>
    </Card>
  </div>
</template>

<script>
import Card from "./components/Card";
import { katas } from "./data.js";
import { ref, computed, watch } from "vue";

export default {
  name: "CApp",
  components: { Card },
  setup() {
    const kataRank = ref("Select Rank");
    const searchTerm = ref("");
    const kataData = ref(katas);

    const updateList = () => {
      kataData.value = katas;
      const filtered = kataData.value.filter(
        (item) => item.kyu === Number(kataRank.value)
      );
      if (filtered.length >= 1) {
        kataData.value = filtered;
      } else {
        kataData.value = katas;
      }
    };

    const CodeCount = computed(() => {
      return kataData.value.length;
    });

    let debounceTimeout;
    watch(
      () => searchTerm.value,
      () => {
        clearTimeout(debounceTimeout);
        debounceTimeout = setTimeout(() => {
          if (searchTerm.value == "") {
            kataData.value = katas;
          }
          const filtered = kataData.value.filter(
            (item) =>
              item.title.toUpperCase().indexOf(searchTerm.value.toUpperCase()) >
              -1
          );
          if (filtered.length > 0) {
            kataData.value = filtered;
          } else {
            kataData.value = katas;
          }
        }, 500);
      }
    );
    return {
      kataRank,
      searchTerm,
      kataData,
      updateList,
      CodeCount,
    };
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
  margin: 10px;
}
.link {
  color: white;
  margin: 5px;
}
h2 {
  color: white;
  padding: 5px 10px;
  border: 2px solid white;
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
  margin: 30px 0;
  padding: 15px 30px;
  outline-color: black;
  color: #2c3e50;
  font-family: "Ribeye", cursive;
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
</style>
