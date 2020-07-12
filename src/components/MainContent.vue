<template>
  <v-container>
    <!-- <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Vuetify
        </h1>

        <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers,
          <br>please join our online
          <a
            href="https://community.vuetifyjs.com"
            target="_blank"
          >Discord Community</a>
        </p>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          What's next?
        </h2>

        <v-row justify="center">
          <a
            v-for="(next, i) in whatsNext"
            :key="i"
            :href="next.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ next.text }}
          </a>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Important Links
        </h2>

        <v-row justify="center">
          <a
            v-for="(link, i) in importantLinks"
            :key="i"
            :href="link.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ link.text }}
          </a>
        </v-row>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <h2 class="headline font-weight-bold mb-3">
          Ecosystem
        </h2>

        <v-row justify="center">
          <a
            v-for="(eco, i) in ecosystem"
            :key="i"
            :href="eco.href"
            class="subheading mx-3"
            target="_blank"
          >
            {{ eco.text }}
          </a>
        </v-row>
      </v-col>
    </v-row> -->
    
    <v-container class="grey lighten-5">
        <transition-group
          tag="div"
          class="row"
          name="list"
        >
          <v-col
            v-for="gif in gifs"
            :key="gif.id"
            xs="12"
            sm="6"
            md="4"
          >
            <v-sheet min-height="250">
              <v-lazy>
                <v-img
                  :src="gif.images.original.url"
                  lazy-src='../assets/preloader.gif'
                />
              </v-lazy>
              <v-btn depressed color="primary" @click="share(gif)">Share</v-btn>
            </v-sheet>
          </v-col>
        </transition-group>
    </v-container>
  </v-container>
</template>

<script>
  export default {
    name: 'MainContent',

    props: [
      'gifs'
    ],

    data: () => ({

    }),

    methods: {
      share: function(gif) {
        navigator.share({
          text: 'GIF',
          url: gif.images.original.url
        })
      }
    },
  }
</script>

<style scoped>
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateY(80px);
}
</style>
