<template>
  <div>

    <div class="section_un">
      <div class="container">
                  <img src="../assets/images/Red and Black Shadow Loud Voices for Justice Logo (1).png">

        <div class="row">
          <h1 class="tittleone">ENVIE DE TESTER VOS CONNAISSANCES CINÉMATOGRAPHIQUE ? </h1>
          <p class="txt_title">
            Bienvenue au quizz ! Une série de question vous sera proposé, vous devrez y répondre par "oui" ou par "non". Essayer de répondre au maximum de question que vous pouvez dans le temps imparti ! Bonne chance !
          </p>
        </div>
        <nuxt-link to="/quizz"><b-button class="btn_start">Go</b-button></nuxt-link>
      </div>
    </div>

   
    

    
      
  </div>
</template>

<script>
export default {
  name: 'IndexPage'
}
</script>
<style scoped>


img{
    display: block;
    margin-left: auto;
    margin-right: auto;
}
.section_un {
  background-image: url("../static/images/black-monochrome-simple-background-texture-circle-light-534895-wallhere.com.jpg");
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

.tittleone {
  color: #fff;
    font-size: 35px;
    text-transform: uppercase;
    font-weight: bold;
    padding-bottom: 19px;
}

.txt_title {
  color: #fff;
  font-size: 20px;
  line-height: 1.3;
}

.btn_start {
  background-color: #e25d5d;
  border-radius: 0px;
  color: rgb(0, 0, 0);
  font-size: 18px;
  padding: 10px 40px;
  margin-top: 30px;
  margin-left: 510px;
}
.btn_start:hover {
  background-color: #fff;
  color: #555;
  border: 1px solid #fff;
}


</style>