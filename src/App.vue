<script setup>
import { ref, onMounted, computed } from 'vue'
import HeaderComp from "./components/template/HeaderComp.vue";
import FooterComp from "./components/template/FooterComp.vue";
const query = ref('')
const my_anime = ref([])
const search_results = ref([])

const my_anime_asc = computed(() => {
	return my_anime.value.sort((a, b) => {
		return a.title.localeCompare(b.title)
	})
})

const searchAnime = () => {
	const url = `https://api.jikan.moe/v4/anime?q=${query.value}`
	fetch(url)
		.then(res => res.json())
		.then(data => {
			search_results.value = data.data
		})
}

const handleInput = (e) => {
	if (!e.target.value) {
		search_results.value = []
	}
}

const addAnime = (anime) => {
	search_results.value = []
	query.value = ''

	my_anime.value.push({
		id: anime.mal_id,
		title: anime.title,
		image: anime.images.jpg.image_url,
		total_episodes: anime.episodes,
		watched_episodes: 0,
	})

	localStorage.setItem('my_anime', JSON.stringify(my_anime.value))
}

const increaseWatch = (anime) => {
	anime.watched_episodes++
	localStorage.setItem('my_anime', JSON.stringify(my_anime.value))
}

const decreaseWatch = (anime) => {
	anime.watched_episodes--
	localStorage.setItem('my_anime', JSON.stringify(my_anime.value))
}

onMounted(() => {
	my_anime.value = JSON.parse(localStorage.getItem('my_anime')) || []
})
</script>

<template>
	<HeaderComp />
	<main>
		<div class="subdiv">
		<form @submit.prevent="searchAnime">
			<input type="text" placeholder="Search for an anime..." v-model="query" @input="handleInput" />
			<button type="submit" class="button">Search</button>
		</form>

		<div class="results" v-if="search_results.length > 0">
			<div v-for="anime in search_results" class="result">
				<div class="re">
				<img :src="anime.images.jpg.image_url" />
				<div class="details">
					<h3>{{ anime.title }}</h3>
					<p :title="anime.synopsis" v-if="anime.synopsis">{{ anime.synopsis.slice(0, 120) }}...</p>
					<span class="flex-1"></span>
					<button @click="addAnime(anime)" class="button">Adicionar no meus Animes</button>
				</div></div>
			</div>
		</div>
		</div>
        <h2>Animes para assistir</h2>
		<div class="myanime" v-if="my_anime.length > 0">
			

			<div v-for="anime in my_anime_asc" class="anime">
				<img :src="anime.image" />
				<h3>{{ anime.title }}</h3>
				<div class="flex-1"></div>
				<span class="episodes">Episódios {{ anime.total_episodes }}</span>
				<input type="checkbox" class="check"> 
			</div>
		 </div>
	</main>
	<FooterComp />
</template>

<style scoped>
#app {
  max-width: auto;
  background-color:#18171D;
  font-family: 'inter', sans-serif;
}
*{
	background-color:#18171D;
	font-family: 'inter', sans-serif;
  }
:root{
    --cor1: #18171D;
    --cor2: #FF0B53;
    --cor3: #312F3E;
    --cor4: #49455E;
    --cor5: #000000;
    --cor6: #FFFFFF;
  }
  
  .anime{
	border: solid 5px #FF0B53;
	margin-bottom: 5px;
	display: flex;
	flex-direction: row;
    border-radius: 20px;
	width: 90%;
  }
  .results, input{
	background-color: #ffffff;
	border-radius: 10px;
    border: #FF0B53 solid 1px;
	border-radius: 5px 0px 0px 5px;

  }
  .episodes,h2,h3{
	color: #FFFFFF;
	font-family: 'inter', sans-serif;
  }
  .anime button{
	background-color: #FF0B53;
	border: none;
	border-radius: 10%;
	color: #FFFFFFff;
	font-weight: bold;
	font-size: 15px;
	width: 20px;
	height: 20px;
	text-align: center;
	margin-right: 3px;
	margin-bottom: 2px;
  }
  form button{
	background-color: #FF0B53;
	color: #FFFFFF;
	border: #FF0B53 solid 1px;
	border-radius: 0px 5px 5px 0px;

  }
  .result{
	border: #FF0B53 2px solid;
	margin-bottom: 5px;
  }
  .subdiv{
	display: flex;
	flex-direction: column;
	margin-left: 1rem;
	margin-right: 1rem;
  }
  .results button{
   border: none;
   background-color: #FF0B53;
   color: #FFFFFF;
   margin-bottom: 1rem;
   margin-left: 5px;
  }
  .re{
	margin: 1rem;
  }
  .results img{
	margin-top: 5px;
	margin-left: 5px;
	border: none;
	border-radius: 10px;
	width: 200px;
	height: 300px;
  }
  .episodes{
	border: solid 1px #FF0B53;
	background-color: #FF0B53;
	font-weight: bold;
	border-width: 5px;
	border-radius: 5px 5px 5px 5px;
	margin-top: 20px;
	width:150px;
	height: 15px;
	align-items: center;
	text-align: center;
	margin-left: 20px;
  }
  .results{
	display: flex;
	flex-direction: row;
  }

  footer{
	margin-top: 1rem;
  }

  .myanime img{
	margin-top: 3px;
	margin-left: 5px;
	border: none;
	border-radius: 10px;
	width: 50px;
	height: 50px;
	}
	
	.check {
	border: #FF0B53 2px solid;
	width: 20px;
	height: 20px;
	margin-top: 22px;
	}
	.myanime{
		
		align-items: center;
		display: flex;
		flex-direction: column;
	}
</style>