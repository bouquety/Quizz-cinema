<template>
  <div class="section_un ">
    <b-modal   centered ref="gameover"  header-class="justify-content-center" title="Game Over" hide-footer hide-header-close no-close-on-backdrop no-close-on-esc>
  
    <div class="d-block text-center">
     <p> Malheureusement le temps imparti est écoulé ! </p>
     <p> Votre score est de {{score}} </p>
     <p> Voulez-vous réessayer ?</p>
    </div>
        <b-button class="mt-3" block v-on:click="nextQuestion()">Recommencer </b-button>
  </b-modal>
  <div class="time_score">
    <div class="score">
   Score : {{score}}
</div>
<div class="time" :style="{'color':color_timer}">
   Temps restant : {{timer}} secondes
</div>
  </div>
    <div class="container">
    
      <div class="row">                  
      <p class="question">
        {{questions}}
      </p>
      </div>
  <div class="image_movie">
<img  class="acteur" :src="acteur_image">
<img  class="movie" :src="poster">
  </div>
      <div class="row" v-if="statut == 'notRespond'">
 
    <b-button class="btn_response" v-on:click="responseQuizz('Oui')">Oui</b-button>
    <b-button class="btn_response" v-on:click="responseQuizz('Non')">Non</b-button>
    
      </div>
     
    <div v-if="result == true && statut == 'respond'">
      <div class="row">
      <b-button class="btn_response" :style="{'background-color':color_oui}" disabled >Oui</b-button>
      <b-button class="btn_response" :style="{'background-color':color_non}" disabled>Non</b-button>
      </div>
      <p class="response_true">
        Super, bonne réponse ;) ! 
      </p>
      <b-button class="btn_response" v-on:click="nextQuestion()">Suivant</b-button>
      </div>
      <div v-if="result == false && statut == 'respond'">
        <div class="row">
      <b-button class="btn_response" :style="{'background-color':color_oui}" disabled >Oui</b-button>
      <b-button class="btn_response" :style="{'background-color':color_non}" disabled>Non</b-button>
      </div>
             <p class="response_false">
        Dommage mauvaise réponse :( ! 
      </p>
      <b-button class="btn_response" v-on:click="nextQuestion()">Suivant</b-button>
      </div>
  
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
      timer:30,
      modal_statut:'notshow',
      color_oui:'',
      color_non:'',
      color_timer:''

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
      console.log(this.movie)
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
          this.color_oui='green'
          this.color_non='red'
          return this.score+=1
        } else {
          this.color_oui='red'
          this.color_non='green'
          return this.result=false
        }
      }
      else {
         if (responseUser == "Oui") {
           this.color_oui='red'
          this.color_non='green'
          return this.result=false
        } else {
          this.color_oui='red'
          this.color_non='green'
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
        this.color_timer='white'
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

      else if (this.timer<=6){
        this.color_timer='red'
         this.timer--
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
  background-image: url(/_nuxt/static/images/black-monochrome-simple-background-texture-circle-light-534895-wallhere.com.jpg);
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
    font-size: 35px;
    line-height: 1.3;
    display: block;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 38px;

}

.response_true{
    color: #28a745;
    font-size: 38px;
    text-align: center;
    font-weight: bold;
}

.response_false{
    color: #ee0000;
    font-size: 38px;
    text-align: center;
    font-weight: bold;
}




.btn_response {
    background-color: blanchedalmond;
    border-radius: 20px;
    color: rgb(0, 0, 0);
    font-size: 26px;
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 6em;
}
.btn_response:hover {
  background-color: #fff;
  color: #555;
  border: 1px solid #fff;
}

.image_movie{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  position: relative;
  overflow: hidden;
   margin-bottom: 36px;
}

.image_movie .movie{
 width: 55%;
    margin: 1rem;
    height: 100%;
       border: 0px solid transparent;
    border: 5px solid;
        border-color: darkgray;


}
.image_movie .acteur{
         width: 23%;
    border: 5px solid;
    border-color: darkgray;
}
.time_score{
      margin-left: 37px;
    margin-bottom: 233px;
    font-size: 36px;
     color: #fff;
    line-height: 1.3;
    width: 19%;
   
}
</style>