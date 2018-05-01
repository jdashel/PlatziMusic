<template>
  <div id="app">
    <img src="../assets/logo.png" alt="Image">
    <h1>PlatziMusic</h1>
    <select v-model="selectedCountry">
      <option v-for="country in countries" :value="country.value">{{ country.name }}</option>
    </select>
    <spinner v-show="loading" />
    <ul>
      <artist v-for="artist in artists" :artist="artist" :key="artist.mbid"/>
    </ul>
  </div>
</template>

<script>
import Artist from './Artist.vue'
import getArtists from '../lastfm'
import Spinner from './Spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'}
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtirsts() {
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then((artists) => {
          this.artists = artists
          this.loading = false
        })
    }
  },
  mounted() {
    this.refreshArtirsts()
  },
  watch: {
    selectedCountry: function() {
      this.refreshArtirsts()
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #42b983;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
