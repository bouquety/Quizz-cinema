<template>
 <div class="section_un">
   <div class="container">
<p class="question">
       did {{acteur_movie[random]}}   play in {{movie_title[random]}}
          </p>
      </div>
          <b-button class="btn_oui">Oui</b-button>
          <b-button class="btn_oui">Non</b-button>

    </div>


</template>

<script>
export default{
    data() {
    this.apikeys= process.env.APIKEY
    return{
        baseUrl : 'https://api.themoviedb.org/3',
        movie:[],
        movie_title:[],
        acteur:[],
        acteur_movie:[],
        random:''
    }
    },
  created(){
      this.initQuizz(), this.getRandomMovie()
  },

    methods: {
        async initQuizz(){
            this.movie = await this.$axios.$get(this.baseUrl+'/movie/popular?api_key='+this.apikeys)
            for (var i = 0; i < this.movie.results.length; i++) {
                this.movie_title.push(this.movie.results[i].title)
                }
                console.log(this.movie)
                this.acteur = await this.$axios.$get(this.baseUrl+'/person/popular?api_key='+this.apikeys)
                 for (var i = 0; i < this.acteur.results.length; i++) {
                   this.acteur_movie.push(this.acteur.results[i].name)
                }
                   console.log(this.acteur)


        },

        responseQuizz(){

        },

        getRandomMovie(){
          let min=0; 
          let max=10;  
          this.random = Math.floor(Math.random() * (max - min)) + min; 
        }
    }
}
</script>
<style scoped>
.container_header {
  margin: 0px;
}
.section_un {
  background-image: url("../../assets/images/black-monochrome-simple-background-texture-circle-light-534895-wallhere.com.jpg");
  height: 100vh;
  display: flex;
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
  font-size: 20px;
  line-height: 1.3;
}
</style>