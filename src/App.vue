<script setup>
import { ref, onMounted, computed } from "vue";
import HeaderComp from "./components/template/HeaderComp.vue";
import FooterComp from "./components/template/FooterComp.vue";
const query = ref("");
const my_anime = ref([]);
const search_results = ref([]);

const my_anime_asc = computed(() => {
  return my_anime.value.sort((a, b) => {
    return a.title.localeCompare(b.title);
  });
});

const searchAnime = () => {
  const url = `https://api.jikan.moe/v4/anime?q=${query.value}`;
  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      search_results.value = data.data;
    });
};

const handleInput = (e) => {
  if (!e.target.value) {
    search_results.value = [];
  }
};

const DeleteAnime = (anime) =>{
 
	
}

const addAnime = (anime) => {
  search_results.value = [];
  query.value = "";

  my_anime.value.push({
    id: anime.mal_id,
    title: anime.title,
    image: anime.images.jpg.image_url,
    total_episodes: anime.episodes,
    watched_episodes: 0,
  });

  localStorage.setItem("my_anime", JSON.stringify(my_anime.value));
};
  
const excluir = (anime_id) =>{
  const index = my_anime_asc.value.findIndex ((a)=> a.id===anime_id);
  my_anime_asc.value.splice (index,1);
}

onMounted(() => {
  my_anime.value = JSON.parse(localStorage.getItem("my_anime")) || [];
});
</script>

<template>
  <HeaderComp />
  <main>
    <div class="subdiv">
      <form @submit.prevent="searchAnime">
        <div class="search">
          <input
            type="text"
            placeholder="Search for an anime..."
            v-model= "query"
            @input="handleInput"
          />
          <button type="submit" class="button">Search</button>
        </div>
      </form>

      <div class="results" v-if="search_results.length > 0">
        <div v-for="anime in search_results" class="result">
          <div class="re">
            <img :src="anime.images.jpg.image_url" />
            <div class="details">
              <h3>{{ anime.title }}</h3>
              <div class="syn"></div><p :title="anime.synopsis" v-if="anime.synopsis">
                {{ anime.synopsis.slice(0, 120) }}...
              </p>
              <span class="flex-1"></span>
              <button @click="addAnime(anime)" class="button">
                Adicionar no meus Animes
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <h2>Animes para assistir</h2>
    <div class="myanime" v-if="my_anime.length > 0">
      <div v-for="anime in my_anime_asc" class="anime">
        <div class="esp">  <img :src="anime.image" />
        <h3>{{ anime.title }}</h3></div>
        <div class="flex-1">
        <span class="episodes">Episódios {{ anime.total_episodes }}</span>
        <input type="checkbox" class="check" />
        <button @click="excluir(anime.id)">excluir</button></div>
      </div>
    </div>
  </main>
  <FooterComp />
</template>

<style scoped>
#app {
  max-width: auto;
  background-color: #18171d;
  font-family: "inter", sans-serif;
  
}
* {
  background-color: #18171d;
  font-family: "inter", sans-serif;
}
:root {
  --cor1: #18171d;
  --cor2: #ff0b53;
  --cor3: #312f3e;
  --cor4: #49455e;
  --cor5: #000000;
  --cor6: #ffffff;
}

.anime {
  border: solid 5px #ff0b53;
  margin-bottom: 5px;
  display: flex;
  flex-direction: row;

  width: 90%;
  justify-content: space-between;
}
form input {
  background-color: #ffffff;
  border-radius: 10px;
  border: #ff0b53 solid 1px;
  border-radius: 5px 0px 0px 5px;
}
.episodes,
h2,
h3 {
  color: #ffffff;
  font-family: "inter", sans-serif;
}
.anime button {
  background-color: #ff0b53;
  border: none;
  border-radius: 10%;
  color: #ffffffff;
  font-weight: bold;
  font-size: 15px;
  width: 20px;
  height: 20px;
  text-align: center;
  margin-right: 3px;
  margin-bottom: 2px;
}
form button {
  background-color: #ff0b53;
  color: #ffffff;
  border: #ff0b53 solid 1px;
  border-radius: 0px 5px 5px 0px;
}
.result {
  border: #ff0b53 4px solid;
  border-radius: 10px;
  margin-bottom: 5px;
  margin-left: 5px;
  align-items: center;

}
.subdiv {
  display: flex;
  flex-direction: column;
  margin-left: 1rem;
  margin-right: 1rem;
}
.results button {
  border: none;
  background-color: #ff0b53;
  color: #ffffff;
  margin-bottom: 1rem;
  margin-left: 5px;

}
.re {
  margin: 1rem;
}
.results img {
  margin-top: 5px;
  margin-left: 5px;
  border: none;
  border-radius: 10px;
  width: 200px;
  height: 300px;
}
.episodes {
  border: solid 1px #ff0b53;
  background-color: #ff0b53;
  font-weight: bold;
  border-width: 5px;
  border-radius: 5px 5px 5px 5px;
  margin-top: 20px;
  width: 150px;
  height: 15px;
  align-items: center;
  text-align: center;
  margin-left: 20px;
}
.results {
  display: grid;
 grid-template-columns:repeat(5,20%);
 margin-left: 7px;
 margin-left: 7px;
 margin-top: 2rem;
 justify-content: center;
}

footer {
  margin-top: 1rem;
}

.myanime img {
  margin-top: 3px;
  margin-left: 5px;
  border: none;
  border-radius: 10px;
  width: 50px;
  height: 50px;
}

.check {
  border: #ff0b53 2px solid;
  width: 20px;
  height: 20px;
  margin-top: 22px;
  margin-left: 1rem;
  margin-right: 1rem;
  background-color: none;
}
.myanime {
  align-items: center;
  display: flex;
  flex-direction: column;
}
.details {
	color:#ffffff;
}
p{
  color: #ffffff;
}

.syn{
  color: #ffffff;
}
.anime button{
  width: 100px;
  margin-top: 1%;
  margin-right: 1rem;
  border-radius:10px ;
}
.myanime h3{
  text-align: left;
  margin-left: 1rem;
}
.esp{
  display: flex;
  flex-direction: row;
}
 h2{
  text-align: center;
  align-items: center;
  align-content: center;
  justify-content: center;
  font-size: 35px;
 }
 form{
  display: grid;
  grid-template-columns: repeat(5,20%);
 }
</style>
