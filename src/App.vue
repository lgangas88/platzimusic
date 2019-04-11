
<template lang='pug'>
  #app
    img(src='https://lgangas88.github.io/platzimusic/dist/logo.png')
    h1 PlatziMusic
    select(v-model='countrySelected')
      option(v-for='country in countries' :value='country.value') {{country.name}}
    spinner(v-show='showSpinner')
    ul
      artist(v-for='artist in artists' :artist='artist' :key='artist.mbid')
</template>

<script>
import getArtists from "./api";
import artist from "./components/Artist.vue";
import spinner from './components/Spinner.vue'

export default {
  name: "app",
  data() {
    return {
      artists: [],
      countries: [
        { name: "Argentina", value: "argentina" },
        { name: "Peru", value: "peru" },
        { name: "Chile", value: "chile" }
      ],
      countrySelected : 'argentina',
      showSpinner : false
    };
  },
  components: {
    artist,
    spinner
  },
  methods : {
    refreshArtist : function () {
      this.showSpinner = true
      this.artists = []
      getArtists(this.countrySelected).then(artists => {
        this.artists = artists
        this.showSpinner = false
      })
    }
  },
  mounted: function() {
    this.refreshArtist()
  },
  watch: {
    countrySelected :function () {
      this.refreshArtist()
    }
  }
};
</script>

<style lang='stylus'>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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
