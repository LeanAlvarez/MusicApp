<template lang="pug">
  #app
    img(src='https://leanalvarez.github.io/MusicApp/dist/logo.png')
    h1 MusicApp
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './API'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina'},
        { name: 'Brasil', value: 'brazil'},
        { name: 'Colombia', value: 'colombia'},
        { name: 'España', value: 'spain'},
        { name: 'Francia', value: 'france'},
        { name: 'Italia', value: 'italy'},
        { name: 'Japon', value: 'japan'},
        { name: 'Uruguay', value: 'uruguay'}
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
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then( function (artists) {
          self.loading = false
          self.artists = artists
        })
    }
  },
  mounted() {
    this.refreshArtist()
  },
  watch: {
    selectedCountry() {
      this.refreshArtist()
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
  color red !important
  margin-top 60px

h1, h2
  font-weight normal
  color #42b983

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983 !important
</style>
