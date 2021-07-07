<template>
  <div class="main">

    <v-progress-circular
      :size="200"
      v-if="!dataLoaded"
      class="loadingCircle ml-10"
      indeterminate
      color="primary"
    ></v-progress-circular>

    <div class="d-flex align-center justify-start pa-6 page" v-else>
      <img class="card-img-top" :src="detailedInfo.Poster" alt="No poster found">
      <div class="card ml-5">

        <div class="card-body">
          <h1 class="card-title">{{ detailedInfo.Title }}</h1>
        </div>
        <ul class="list-group list-group-flush">
          <li class="list-group-item">Release Date : {{ detailedInfo.Released }}</li>
          <li class="list-group-item">Director : {{ detailedInfo.Director }}</li>
          <li class="list-group-item">Actors : {{ detailedInfo.Actors }}</li>
          <li class="list-group-item">Genre : {{ detailedInfo.Genre }}</li>
          <li class="list-group-item">Plot : {{ detailedInfo.Plot }}</li>


        </ul>
        <div class="card-body ml-5">
          <v-btn elevation="10" :href="imdbLink " class="card-link mt-5 ">IMDB Page</v-btn>

        </div>
      </div>
    </div>
  </div>

</template>

<script>
import axios from "axios";

export default {
  name: "detail",
  data() {
    return {
      dataLoaded: false,
      detailedInfo: [],
      movieID: '',
      errorMessage: '',
      url: 'http://www.omdbapi.com/?',
      apiKey: '&apikey=6448bbcf',
    }
  },

  mounted() {
    this.movieID = this.$route.params.id
    this.url += 'i=' + this.movieID + this.apiKey
    this.imdbLink += this.movieID.id
    this.dataLoaded = false
    this.getData(this.url)

  },


  methods: {
    async getData(newUrl) {
      const response = await axios.get(newUrl);
      this.detailedInfo = response.data
      console.log(this.$route.redirectedFrom)
      this.dataLoaded = true;
    },


  }
}
</script>

<style scoped>

.page {
  height: 85vh;
  width: 100%;

}

.main {


}

.main {
  height: 100%;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: center;
  justify-content: center;
}
</style>
