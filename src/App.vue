<template>
  <v-app>
    <v-overlay v-if="isFetching">
    <v-progress-circular
      :size="50"
      color="primary"
      indeterminate
    ></v-progress-circular>
    </v-overlay>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="logo"
          class="shrink mr-2"
          contain
          src="./assets/logo.png"
          width="40"
        />
      </div>
      <v-form>
        <v-text-field
          v-model="searchText"
          label="Search"
          hide-details="true"
          @input="search"
        ></v-text-field>
      </v-form>
    </v-app-bar>

    <v-main>
      <v-img
        v-if="notFound"
        :src="notFoundURL"
      />
      <MainContent v-bind:gifs="gifs"/>
      <v-btn v-if="loadMore" depressed color="primary" @click="loadMoreGiphs">Load More</v-btn>
    </v-main>
  </v-app>
</template>

<script>
import MainContent from './components/MainContent';

export default {
  name: 'App',

  components: {
    MainContent,
  },

  data: () => ({
    isFetching: false,
    gifs: [],
    searchText: '',
    notFound: false,
    notFoundURL: '',
    loadMore: false,
    counterForPagination: 0,
  }),

  created: function() {
    const getGiphs = () => {
      this.isFetching = true;
      fetch('https://api.giphy.com/v1/gifs/trending?api_key=PhRv8VCfrXRbCSS12cHSopUs4hu7OuSr')
      .then(response => response.json())
      .then(data => {
        data.data.forEach(element => {
          this.gifs.push(element)
        });
      })
      .then(() => {
        this.isFetching = false;
      })
      .catch(err => {
        console.log(err)
      })
    }
    getGiphs()
    const getRandomNotFound = () => {
      fetch('https://api.giphy.com/v1/gifs/random?api_key=PhRv8VCfrXRbCSS12cHSopUs4hu7OuSr&tag=notFound')
      .then(response => response.json())
      .then(data => {
        this.notFoundURL = data.data.images.original.url;
      })
      .catch(err => {
        console.log(err)
      })      
    }
    getRandomNotFound()
  },

  methods: {
    search: function() {
      this.isFetching = true;
      fetch(`https://api.giphy.com/v1/gifs/search?q=${this.searchText}&api_key=PhRv8VCfrXRbCSS12cHSopUs4hu7OuSr&offset=0`)
      .then(response => response.json())
      .then(data => {
        if(data.data.length) {
          this.notFound = false;
        } else {
          this.notFound = true
        }
        let newGiphs = []
        data.data.forEach(element => {
          newGiphs.push(element)
        });
        this.gifs = newGiphs;
        if (data.pagination.total_count > this.gifs.length) {
          this.loadMore = true;
        } else {
          this.loadMore = false;
        }
      })
      .then(() => {
        this.isFetching = false;
      })
      .catch(err => {
        console.log(err)
      })
    },
    loadMoreGiphs: function() {
      this.isFetching = true;
      fetch(`https://api.giphy.com/v1/gifs/search?q=${this.searchText}&api_key=PhRv8VCfrXRbCSS12cHSopUs4hu7OuSr&offset=${this.counterForPagination+25}`)
      .then(response => response.json())
      .then(data => {
        data.data.forEach(element => {
          this.gifs.push(element)
        });
        if (data.pagination.total_count > this.gifs.length) {
          this.loadMore = true;
        } else {
          this.loadMore = false;
        }
        this.counterForPagination = this.counterForPagination + 25;
      })
      .then(() => {
        this.isFetching = false;
      })
      .catch(err => {
        console.log(err)
        })
    }
  }
};
</script>
<style scoped>
form {
  width: 100%;
}
</style>