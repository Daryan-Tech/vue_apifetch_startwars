<template>
  <div id="app">
    <div id="characterTabs">
      <ul>
        <li v-for="(character, index) in characters" :key="character.id">
          <a v-on:click="activeTab=index" v-bind:class="[ activeTab === index ? 'active' : '' ]">
            <img :src="character.image" alt="character image">
          </a>
        </li>
      </ul>
    </div>
    

    <div id="content">
      <div v-for="(character, index) in characters" :key="character.id">
          <character-movies :class="activeTab === index ? 'display' : 'dontDisplay' "  :apiUrl="character.apiLink" :tabActive="activeTab === index ? true : false"></character-movies>
      </div>
    </div>


  </div>
</template>


<script>
  import uuidv4 from 'uuid/v4';
  import CharacterMovies from './components/characterMovies';

  export default {
    name: 'app',
    components: {
      'character-movies': CharacterMovies
    },
    data: function() {
      return {
        activeTab: 0,
        errors: null,
        characters: [
          {
            id: uuidv4(),
            image: require('@/assets/images/LukeSkywalker.jpg'), 
            apiLink: 'https://swapi.co/api/people/1/',
          },
          {
            id: uuidv4(),
            image:  require('@/assets/images/R2d2.jpg'), 
            apiLink: 'https://swapi.co/api/people/3/',
          },
          {
            id: uuidv4(),
            image:  require('@/assets/images/DarthVader.jpeg'), 
            apiLink: 'https://swapi.co/api/people/4/',
          }],
      }
    },
  }
</script>

