<template>
  <div class="section_un">
    <b-modal   centered ref="gameover"  header-class="justify-content-center" title="Game Over" hide-footer hide-header-close no-close-on-backdrop no-close-on-esc>
    <!-- <div class="modal-header text-center">
  <h3 class="modal-title w-100">Quiz Results</h3>
</div> -->
    <div class="d-block text-center">
     <p> Malheureusement le temps imparti est écoulé ! </p>
     <p> Votre score est de {{score}} </p>
     <p> Voulez-vous réessayer ?</p>
    </div>
        <b-button class="mt-3" block v-on:click="nextQuestion()">Recommencer </b-button>
  </b-modal>
    <div class="score">
   Score : {{score}}
</div>
<div class="time">
   Temps restant : {{timer}}
</div>
    <div class="container">
      <div class="row">                  
      <p class="question">
        {{questions}}
      </p>
      </div>
      <div class="row" v-if="statut == 'notRespond'">
    <b-button class="btn_response" v-on:click="responseQuizz('Oui')">Oui</b-button>
    <b-button class="btn_response" v-on:click="responseQuizz('Non')">Non</b-button>
    </div>
     <div class="row" v-else>
    <b-button class="btn_response" disabled>Oui</b-button>
    <b-button class="btn_response" disabled>Non</b-button>
    </div>
    <div v-if="result == true && statut == 'respond'">
      <p class="response_true">
        Super bonne réponse ;) ! 
      </p>
      <b-button class="btn_response" v-on:click="nextQuestion()">Suivant</b-button>
      </div>
      <div v-if="result == false && statut == 'respond'">
             <p class="response_false">
        Dommage Mauvaise réponse :( ! 
      </p>
      <b-button class="btn_response" v-on:click="nextQuestion()">Suivant</b-button>
      </div>
  </div>
  <div class="row">
<img :src="poster">
<img :src="acteur_image">
  </div>
  </div>
</template>

<script>
export default {
  data() {
    this.apikeys = process.env.APIKEY;
    return {
      baseUrl: "https://api.themoviedb.org/3",
      movie: [],
      movie_title: [],
      acteur: [],
      acteur_movie: [],
      acteur_poster:[],
      movie_poster:[],
      poster:'',
      acteur_image:'',
      random: "",
      id: [],
      movie_id: [],
      score:0,
      result:'',
      questions:'',
      statut:'notRespond',
      timer:30000,
      modal_statut:'notshow'

    };
  },
  created() {
    this.initQuizz(), this.getRandomMovie(), this.getTimer()
  },

  methods: {
    async initQuizz() {
      this.movie = await this.$axios.$get(
        this.baseUrl + "/movie/popular?api_key=" + this.apikeys
      )
      for (var i = 0; i < this.movie.results.length; i++) {
        this.movie_title.push(this.movie.results[i].title)
        this.movie_id.push(this.movie.results[i].id)
        this.movie_poster.push(this.movie.results[i].backdrop_path)
      }
      this.acteur = await this.$axios.$get(
        this.baseUrl + "/person/popular?api_key=" + this.apikeys
      )

      for (var i = 0; i < this.acteur.results.length; i++) {
        this.acteur_movie.push(this.acteur.results[i].name)
        this.acteur_poster.push(this.acteur.results[i].profile_path)
      }
      this.poster= "https://image.tmdb.org/t/p/original"+ this.movie_poster[this.random]
      this.acteur_image= "https://image.tmdb.org/t/p/original"+ this.acteur_poster[this.random]
      this.questions= this.acteur_movie[this.random]+" à joué dans "+ this.movie_title[this.random] +" ?"
        
     
    },
    

    async responseQuizz(responseUser) {
      let responseResultat
      this.statut='respond'
      this.id = await this.$axios.$get(
        this.baseUrl +
          "/movie/" +
          this.movie_id[this.random] +
          "/credits?api_key=" +
          this.apikeys
      );
      for (var i = 0; i < this.id.cast.length; i++) {
        if (this.acteur_movie[this.random] == this.id.cast[i].name) {
           responseResultat = true;
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
      let max = 20;
      this.random = Math.floor(Math.random() * (max - min)) + min;
    },

    nextQuestion(){
      if (this.modal_statut == 'show'){
        this.modal_statut='notshow'
        this.$refs['gameover'].hide()
        this.statut='notRespond'
        this.score=0
        this.timer=30
        this.getRandomMovie()
      this.initQuizz()
      }
      else{
        this.statut='notRespond'
        this.getRandomMovie()
        this.initQuizz()
      }
    },

    getTimer(){
    
     
        setInterval(() => {
            if (this.timer <= 0){
        clearInterval()
        this.getGameOver()
      }
      else {
        this.timer--
      }
        },1000)
        
      
    },
    getGameOver(){
       this.$nextTick(()=>{
     this.$refs['gameover'].show()
      this.modal_statut='show'
  });
     
    }
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