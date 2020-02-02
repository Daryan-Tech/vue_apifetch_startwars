<template>
  <div id="charaterMovies">
    <div v-if="loading">
      <img  id="loading_gif" :src="require('@/assets/images/loading.gif')" alt="loadingGIF" />
    </div>
    <ul>
      <li v-for="movie in movies" :key="movie.episode_id">
        {{ movie.title }} - {{ format_date(movie.release_date) }}
      </li>
    </ul>
  </div>
</template>


<script>
  import axios from 'axios';
  import moment from 'moment';

  const custom_sort = (a, b) => moment(a.release_date).format('YYYYMMDD') - moment(b.release_date).format('YYYYMMDD');

  export default {
    name: 'character-movies',
    props: ['apiUrl'],
    data: function() {
      return {
        loading: true,
        errors: null,
        movies: [],
      }
    },

    methods: { 
      format_date(value){
        if (value) {
          return moment(String(value)).format('YYYY')
          }
      },
    },

    mounted: function () {
      var characterMovies = [];
        axios
        .get(this.apiUrl)
        .then(response => {
          response.data.films.map(filmApiUrl => {
            axios
              .get(filmApiUrl)
              .then(res => {
                characterMovies.push(res.data);
                this.movies = characterMovies.sort(custom_sort);
                this.loading = false;
              }).catch(e => {
                this.errors = e;
              });
          });
        }).catch(e => {
          this.errors = e;
        });
    }
  }
</script>

