<template lang="pug">
  #app
    img(src='https://vicargo.github.io/vue-lastfm/dist/logo.png')
    h1 Vue LastFM
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue' //al ser index.js no hace falta poner la ruta entera. 
import Spinner from './components/Spinner.vue' //al ser index.js no hace falta poner la ruta entera. 
import getArtists from './api' //al ser index.js no hace falta poner la ruta entera. 

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
    refreshArtist(){
      const self = this; //En esta scope this hace referencia al componente. 
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists){
          //this scope global,
          self.loading = false
          self.artists = artists
          
        })
    }
  },
  mounted() {
    this.refreshArtist();
  },
  watch: {
    selectedCountry(){
      this.refreshArtist();
    }
  }
}
</script>

<style lang="stylus">
	#app
		font-family 'Avenir', Helvetica, Arial, sans-serif
		-webkit-font-smoothing antialiased
		-moz-osx-font-smoothing grayscale
		text-align center
		color #2c3e50
		margin-top 60px

	h1, h2
		font-weight normal

	ul
		list-style-type none
		padding 0

	li
		display inline-block
		margin 0 10px

	a
		color #42b983
</style>
