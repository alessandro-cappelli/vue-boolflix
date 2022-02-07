<template>
  <div>
    <input type="text" v-model="inputCerca">
    <button @click="cerca">Cerca</button>
    <ul
        v-for="(element, index) in arrayFilmsSerie" 
        :key="index">
        <li><img class="poster" :src="`https://image.tmdb.org/t/p/original/`+element.poster_path"></li>
        <li><span>Titolo:</span> {{element.title}}</li>
        <li><span>Titolo originale:</span> {{element.original_title}}</li>
        <li><img :src="require(`../assets/imgFlag/${getFlag(element.original_language)}.png`)" alt=""></li>
        <li><span>Voto:</span> {{element.vote_average}}</li>
        <!-- <li v-for="(elem, index) in starsApp" :key="index">*</li> -->
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Main',
    data() {
        return{
          inputCerca: "",
          arrayFilms: [],
          arraySerie: [],
          // starsApp: null,
        }
    },
    computed: {
      arrayFilmsSerie(){
        return [...this.arrayFilms, ...this.arraySerie];
      }
    },
    methods:{
      // getStar: function(voto){
      //   this.starsApp = Math.ceil(voto/2);
      // },
      getFlag: function(language){
        let urlFlag = "jolly";
        if(['it', 'en'].includes(language)){
          urlFlag = language;
        }
        return urlFlag;
      },

      cerca: function(){
          this.cercaFilm();
          this.cercaSerie();
      },

      cercaFilm: function(){
        axios
          .get('https://api.themoviedb.org/3/search/movie', 
          {
            params: {
              api_key: 'dcad4b553c14bd5d5552daeaa7f4d1f0',
              query: this.inputCerca
            }
          })
          .then( (response) => {
            this.arrayFilms = response.data.results;
            console.log('Risposta GET films')
          })
          .catch(function (error) {
            console.log(error);
          });
      },
      
      cercaSerie: function(){
        axios
          .get('https://api.themoviedb.org/3/search/tv?', 
          {
            params: {
              api_key: 'dcad4b553c14bd5d5552daeaa7f4d1f0',
              query: this.inputCerca
            }
          })
          .then( (response) => {
            this.arraySerie = response.data.results;
            console.log('Risposta GET films')
          })
          .catch(function (error) {
            console.log(error);
          });
      }
    }

        
}
</script>

<style lang='scss' scoped>
  ul{
    padding: 0;
    text-align: center;
    background-color: #a2acb5;
    border-radius: 15px;
  }

  li{
    list-style: none;
  }

  .poster{
    width: 170px;
  }

  span{
    font-weight: bold;
  }
</style>