<template>
  <div class="section_un">
    <div class="score">
   Score : {{score}}
</div>
<div class="time">
   timer : 1:25
</div>
    <div class="container">
      <div class="row">                  
      <p class="question">
        {{ acteur_movie[random] }} à joué dans {{ movie_title[random] }} ?
      </p>
      </div>
      <div class="row">
    <b-button class="btn_response" v-on:click="responseQuizz('Oui')">Oui</b-button>
    <b-button class="btn_response" v-on:click="responseQuizz('Non')">Non</b-button>
    </div>
    <div v-if="result === true">
      <p class="response_true">
        Super bonne réponse ;) ! 
      </p>
      <b-button class="btn_response" v-on:click="responseQuizz('Suivant')">Suivant</b-button>
      </div>
      <div v-if="result === false">
             <p class="response_false">
        Dommage Mauvaise réponse :( ! 
      </p>
      <b-button class="btn_response" v-on:click="responseQuizz('Suivant')">Suivant</b-button>
      </div>
  </div>
<img :src="'{{poster}}'">
  </div>
</template>

<script>
export default {
  data() {
    this.apikeys = process.env.APIKEY;
    return {
      baseUrl: "https://api.themoviedb.org/3",
      UrlImage: "",
      movie: [],
      movie_title: [],
      acteur: [],
      acteur_movie: [],
      movie_poster:[],
      poster:'',
      random: "",
      id: [],
      movie_id: [],
      score:0,
      result:''
    };
  },
  created() {
    this.initQuizz(), this.getRandomMovie();
  },

  methods: {
    async initQuizz() {
      this.movie = await this.$axios.$get(
        this.baseUrl + "/movie/popular?api_key=" + this.apikeys
      )
      console.log(this.movie)
      for (var i = 0; i < this.movie.results.length; i++) {
        this.movie_title.push(this.movie.results[i].title)
        this.movie_id.push(this.movie.results[i].id)
        this.movie_poster.push(this.movie.results[i].backdrop_path)
      }
      this.acteur = await this.$axios.$get(
        this.baseUrl + "/person/popular?api_key=" + this.apikeys
      );
      for (var i = 0; i < this.acteur.results.length; i++) {
        this.acteur_movie.push(this.acteur.results[i].name);
      }
      this.poster= "https://image.tmdb.org/t/p/original"+ this.movie_poster[this.random]
      console.log(this.poster)
    },
    

    async responseQuizz(responseUser) {
      let responseResultat
      this.id = await this.$axios.$get(
        this.baseUrl +
          "/movie/" +
          this.movie_id[this.random] +
          "/credits?api_key=" +
          this.apikeys
      );
      for (var i = 0; i < this.id.cast.length; i++) {
        if (this.acteur_movie[this.random] == this.id.cast[i].name) {
          return responseResultat = true;
        }
      }
      if (responseResultat == true){
        if (responseUser == "Oui") {
          this.result=true
          return this.score+=1
        } else {
          return this.result=false
        }
      }
      else {
         if (responseUser == "Oui") {
          return this.result=false
        } else {
          this.result=true
          return this.score+=1
        }
      }
    },

    getRandomMovie() {
      let min = 0;
      let max = 10;
      this.random = Math.floor(Math.random() * (max - min)) + min;
    },
  },
};
</script>
<style scoped>

.section_un {
  background-image: url(/_nuxt/assets/images/black-monochrome-simple-background-texture-circle-light-534895-wallhere.com.jpg);
    height: 100vh;
    
    justify-content: center;
    align-items: center;
    background-size: cover;
    background-attachment: fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    image-rendering: -webkit-optimize-contrast;
}

.question {
 color: #fff;
    font-size: 57px;
    line-height: 1.3;
    display: block;
    margin-left: auto;
    margin-right: auto;

}

.response_true{
    color: #28a745;
    font-size: 38px;
    margin-left: 272px;
}

.response_false{
    color: #ee0000;
    font-size: 38px;
    margin-left: 272px;
}

.score {
 color: #fff;
    font-size: 57px;
    line-height: 1.3;
   
}
.time {
 color: #fff;
    font-size: 57px;
    line-height: 1.3;
   
}

.btn_response {
      background-color: #e25d5d;
    border-radius: 20px;
    color: rgb(0, 0, 0);
    font-size: 59px;
    padding: 10px 40px;
    margin-left: 231px;
    display: block;
}
.btn_response:hover {
  background-color: #fff;
  color: #555;
  border: 1px solid #fff;
}
</style>