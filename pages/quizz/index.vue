<template>
 <div class="section_un">
      <div class="container">

        
      </div>
    </div>
</template>

<script>
export default{
    data() {
    this.apikeys= process.env.APIKEY
    return{
        baseUrl : 'https://api.themoviedb.org/3/discover/movie',
        movie:{},
        movie_title:[]
    }
    },
  mounted(){
      this.initQuizz()
  },

    methods: {
        async initQuizz(){
            this.movie = await this.$axios.$get(this.baseUrl+'?api_key='+this.apikeys)
            for (var i = 0; i < this.movie.results.length; i++) {
                this.movie_title.push(this.movie.results[i].title)
                }
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
</style>