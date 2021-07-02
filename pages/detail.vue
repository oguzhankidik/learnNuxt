<template>

  <div>

    <div class="card" style="width: 18rem;">
      <img class="card-img-top" :src="detailedInfo.Poster" alt="Poster can't be found.">
      <div class="card-body">
        <h5 class="card-title">{{ detailedInfo.Title }}</h5>
        <p class="card-text">
          Release Date : {{detailedInfo.Released}}<br>
          Director : {{detailedInfo.Director}}<br>
          Actors : {{detailedInfo.Actors}}
        </p>

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
      detailedInfo: [],
      test: '',
      url: 'http://www.omdbapi.com/?',
      apiKey: '&apikey=6448bbcf',
    }
  },
  props: {
    selectedItem: {}
  },
  mounted() {
    this.test = this.$route.params
    this.url += 'i=' + this.test.id + this.apiKey
    this.getData(this.url)
  },
  methods: {
    async getData(newUrl) {
      const response = await axios.get(newUrl);
      this.detailedInfo = response.data

    },
  }
}
</script>

<style scoped>

</style>
