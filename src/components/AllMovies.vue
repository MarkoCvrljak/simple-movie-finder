<template>
  <v-container v-if="movieIndex < popularMovies.length" class="grey lighten-5">
    <v-row>
      <v-col :key="movieIndex" v-for="movieIndex in moviesToShow" cols="12" md="4">
        <v-card 
        @click="loadRouterLink(popularMovies[movieIndex].id)"
        style="cursor:pointer"
        class="mx-auto" max-width="400">
          <v-img
            class="white--text align-end"
            height="200px"
            :src="picBasePath + popularMovies[movieIndex].poster_path"
          >
            <v-card-title>{{popularMovies[movieIndex].title}}</v-card-title>
          </v-img>

          <v-card-subtitle class="pb-0 vote">
            <span style="font-weight:700"></span>
            {{popularMovies[movieIndex].vote_average}}
          </v-card-subtitle>

          <v-card-text class="text--primary">
            <p style="padding:20px 10px 0 0">
              <u>release date: {{popularMovies[movieIndex].release_date}}</u>
            </p>
            <div>
              <span style="font-weight:700">DESCRIPTION:</span>
              {{popularMovies[movieIndex].overview.substring(0,110)+"..."}}
            </div>
            <span>({{popularMovies[movieIndex].original_language}})</span>
          </v-card-text>
          <v-card-actions>
            <router-link
              :to="{ name: 'MovieDetails', params: { id: popularMovies[movieIndex].id }}"
            >VIEW MORE</router-link>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <div style="text-align:center; margin-top:30px;">
      <v-btn @click="moviesToShow += 6" color="orange">LOAD MORE</v-btn>
    </div>

    <v-row justify="center">
      <v-dialog v-model="dialog" scrollable max-width="300px">
        <template v-slot:activator="{ on }">
          <v-btn id="floating-btn" color="primary" dark v-on="on">FIND BY GENRE</v-btn>
        </template>
        <v-card>
          <v-card-title>Select Genre</v-card-title>
          <v-divider></v-divider>
          <v-card-text style="height: 300px;">
            <v-radio-group v-model="genreId" column>
              <v-radio label="Drama" value="18"></v-radio>
              <v-radio label="Comedy" value="35"></v-radio>
              <v-radio label="Action" value="28"></v-radio>
              <v-radio label="Animation" value="16"></v-radio>
            </v-radio-group>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn color="blue darken-1" text @click="selectedGenre">Roll</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      picBasePath: "http://image.tmdb.org/t/p/w500/",
      moviesToShow: 6,
      movieIndex: null,
      genreId: null,
      dialog: false
    };
  },
  methods: {
    selectedGenre() {
      const genreNum = parseInt(this.genreId);
      this.$store.commit("setGenreId", genreNum);
      this.$router.push("/movie-roulette");
    },
    loadRouterLink(movieId) {
      this.$router.push({ name: "MovieDetails", params: { id: movieId } });
    }
  },
  computed: {
    popularMovies() {
      return this.$store.getters.getPopularMovies;
    }
  }
};
</script>

<style lang="scss">
@import "../scss/_variables";

.vote {
  position: absolute;
  top: 0px;
  right: 0;
  background: #1976d1;
  color: white !important;
  font-weight: 900 !important;
  padding: 3px !important;
}
#floating-btn {
  position: fixed;
  right: 50px;
  bottom: 12px;
  background: $btn-color;
  color: aliceblue;
}
</style>>

