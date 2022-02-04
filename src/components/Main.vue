<template>
  <div>
    <input type="text" v-model="inputCerca">
    <button @click="cerca">Cerca</button>
    <ul>
      <li 
        v-for="(element, index) in arrayFilms" 
        :key="index">
        <p>{{element.title}}</p>
        <p>{{element.original_title}}</p>
        <p>{{element.original_language}}</p>
        <p>{{element.vote_average}}</p>
      </li>
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
          arrayFilms: []
        }
    },
    methods: {
      cerca: function(){
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
          }
        }
}
</script>

<style>

</style>