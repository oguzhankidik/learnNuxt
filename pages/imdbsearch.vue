<template>

  <div class="pa-6">

    <div class="row no-gutters">
      <div class="col">
        <v-text-field
          outlined
          v-model="searchParams.s"
          label="enter a title"
        ></v-text-field>
      </div>
      <div class="col ml-5">
        <v-text-field
          outlined
          v-model="searchParams.y"
          label="enter a year"
        ></v-text-field>
      </div>
      <div class="col ml-5">
        <v-select
          outlined
          v-model="searchParams.type"
          :items="typeOption"
          label="select a type"

        >
        </v-select>
      </div>
      <div class="col-1 ml-4">
        <v-btn
          outlined

          rounded
          x-large
          @click="searchClick"
        >Search
        </v-btn>
      </div>
    </div>


    {{ errorMessage }}
    <imdb-search-card v-if="imdbResults.Response=='True'" class="movies ml-10 mt-4"
                      :selectedItem="imdbResults.Search"/>

    <div v-if="imdbResults.totalResults>5">
    <v-pagination

      circle
      v-model="pageCount"
      @input="changePage"
      class="my-3"
      :length="pages.length"
      total-visible="5"
    ></v-pagination>
    </div>
  </div>
</template>


<script>


import axios from "axios";

import imdbSearchCard from "~/components/imdb-search-components/imdb-search-card";

export default {

          //hızlı sayfa değişince patlıyor
          //kod gereksiz uzun ?
  components: {imdbSearchCard},
  data() {
    return {
      pageCount: 1,
      imdbResults: [],
      dataLoaded: false,
      apiKey: '&apikey=6448bbcf',
      typeOption: [
        "movie", "series", "short"
      ],
      pages: [],
      errorMessage: '',
      searchParams: {'s': '', 'y': '', 'type': '', 'page': 1},
      finalUrl: 'https://omdbapi.com/?',
    }
  },

  methods: {
    async getData(newUrl) {

      const response = await axios.get(newUrl);
      this.imdbResults = response.data
      this.generateErrorMessage()
      this.dataLoaded = true
    },

    dropdownPageNumber() {
      this.pages = []
      if(this.imdbResults.totalResults>100){
          this.a=10;
      }
      else this.a=Math.ceil(this.imdbResults.totalResults / 10)
      for (let i = 0; i < this.a; i++) {
        this.pages[i] = i + 1;
      }
    },

    changePage() {
      this.finalUrl = 'https://omdbapi.com/?'
      this.searchParams.page=this.pageCount
      this.generateFinalUrl()
      this.getData(this.finalUrl)


    },
    generateErrorMessage() {
      if (this.imdbResults.Error === "Incorrect IMDb ID.") {
        this.errorMessage = 'Please Enter a Title'
      } else {
        this.errorMessage = this.imdbResults.Error
      }
    },

    searchClick() {
      this.searchParams.page = 1
      this.pageCount = 1
      this.finalUrl = 'https://omdbapi.com/?'
      this.generateFinalUrl()
      this.getData2(this.finalUrl)
    },
    async getData2(newUrl) {
      const response = await axios.get(newUrl);
      this.imdbResults = response.data
      this.generateErrorMessage()
      this.dropdownPageNumber()
    },

    generateFinalUrl(){
      let params = {
        s: this.searchParams.s,
        y: this.searchParams.y,
        type: this.searchParams.type,
        page: this.searchParams.page
      }
      let data = new URLSearchParams(params)
      this.finalUrl += data + this.apiKey
    },



  }
}
</script>
<style scoped>

</style>
