<template>

  <div class="pa-6">

    <div class="row no-gutters">
      <div class="col  col-lg col-md ml-5  col-sm-12">
        <v-text-field
          outlined
          v-model="searchParams.s"
          label="enter a title"
        >
        </v-text-field>
      </div>
      <div class="col col-sm-12  col-lg col-md ml-5">
        <v-text-field
          outlined
          v-model="searchParams.y"
          label="enter a year"
        ></v-text-field>
      </div>
      <div class="col col-sm-12 col-lg col-md ml-5">
        <v-select
          outlined
          v-model="searchParams.type"
          :items="typeOption"
          label="select a type"

        >
        </v-select>
      </div>
      <div class="col-1 col-sm-12 col-lg-1 col-md  ml-4">
        <v-btn
          outlined
          :disabled="clicked"
          rounded
          x-large
          @click="searchClick"
        >Search
        </v-btn>
      </div>
    </div>
    <div class="mainScreen">
      <v-progress-linear
        v-if="finalUrl.length>30&&!dataLoaded"
        :size="200"

        class="loadingCircle"

        indeterminate
        color="yellow darken-2"
      ></v-progress-linear>
      <div  v-else>
        <imdb-search-card class="movies mt-4"
                          :selectedItem="imdbResults.Search"/>


        <div v-if="imdbResults.totalResults>10">
          <v-pagination

            circle
            v-model="pageCount"
            @input="changePage"
            class="my-3"
            :length="pages"
            total-visible="7"
          ></v-pagination>
        </div>
      </div>
    </div>
  </div>
</template>


<script>


import axios from "axios";

import imdbSearchCard from "~/components/imdb-search-components/imdb-search-card";

export default {

  components: {imdbSearchCard},
  data() {
    return {
      searchClicked: false,
      pageCount: 1,
      imdbResults: [],
      clicked:false,
      dataLoaded: false,
      apiKey: '&apikey=6448bbcf',
      typeOption: [
        "movie", "series", "short"
      ],
      pages: 0,
      errorMessage: '',
      searchParams: {'s': '', 'y': '', 'type': '', 'page': 1},
      finalUrl: 'https://omdbapi.com/?',
    }
  },

  mounted() {

    if (this.$route.query.s !== undefined)
      this.searchParams.s = this.$route.query.s
    if (this.$route.query.y !== undefined)
      this.searchParams.y = this.$route.query.y
    if (this.$route.query.type !== undefined)
      this.searchParams.type = this.$route.query.type
    this.pageCount = parseInt(this.$route.query.page)

    parseInt(this.$route.query.page)
    if (Object.keys(this.$route.query).length !== 0) {
      let abc = new URLSearchParams(this.$route.query)
      this.finalUrl += abc + this.apiKey
      this.getData(this.finalUrl)
    }
  },


  methods: {
    async getData(newUrl) {

      const response = await axios.get(newUrl);
      this.imdbResults = response.data
      this.generateErrorMessage()
      this.dataLoaded = true
      this.pages = Math.ceil(this.imdbResults.totalResults / 10)

    },
    changePage() {
      this.finalUrl = 'https://omdbapi.com/?'
      this.searchParams.page = this.pageCount
      this.generateFinalUrl()
      this.dataLoaded=false


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
      this.dataLoaded = false

      this.clicked=true
      setTimeout(function(){
        this.clicked = false;
      }.bind(this),650);
    },

    generateFinalUrl() {

      let data = {
        s: this.searchParams.s,
        y: this.searchParams.y,
        type: this.searchParams.type,
        page: this.searchParams.page
      }
      this.$router.push({
        query: data
      })
      this.abc = new URLSearchParams(data)
      this.abc += ""
      this.finalUrl += this.abc + this.apiKey
      this.getData(this.finalUrl)
    },


  }
}
</script>
<style scoped>
.movies {
  margin: auto;
}

.loadingCircle {
  width:90% !important;
}
.mainScreen{
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: center;
}
</style>
