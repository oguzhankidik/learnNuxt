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
    <imdb-search-card v-if="imdbResults.Response=='True'" :max="max" :min="min" class="mt-4"
                      :selectedItem="imdbResults.Search"/>
    <div v-if="imdbResults.totalResults>5" class="d-flex justify-end align-center ">
      <v-btn
        class="pageButtons mt-1"
        @click="pageDown"
        plain
      >
        <v-icon
        >
          mdi-arrow-left
        </v-icon>
      </v-btn>
      <div class="pageCounter">

        <select
          class="form-select ml-2"
          @change="changePage"
          v-model=pageCount>

          <option
            v-for="(item) in pages"
            >
            {{ item }}
          </option>
        </select>
      </div>
      <v-btn
        class="pageButtons mt-1"
        plain
        @click="pageUp"
      >
        <v-icon
          dark
        >
          mdi-arrow-right
        </v-icon>
      </v-btn>
    </div>
    <b-pagination
      v-model="pageCount"
      total-rows="2"
      aria-controls="my-table"
    ></b-pagination>
  </div>
</template>


<script>


import axios from "axios";

import imdbSearchCard from "~/components/imdb-search-components/imdb-search-card";

export default {
          //hızlı sayfa değişince patlıyor
          //sayfayı gösteren select kötü
          //kod gereksiz uzun ?
  components: {imdbSearchCard},
  data() {
    return {
      pageCount: 1,
      firstPage: false,
      secondPage: false,

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
      min: 0,
      max: 5,
    }
  },

  methods: {
    async getData(newUrl) {
      const response = await axios.get(newUrl);
      this.imdbResults = response.data
      this.generateErrorMessage()

      if (this.firstPage) {
        this.setMinMaxFirst()
        this.firstPage = false
      }
      if (this.secondPage) {
        this.setMinMaxSecond()
        this.secondPage = false
      }

      this.dataLoaded = true
    },

    dropdownPageNumber() {
      this.pages = []
      if(this.imdbResults.totalResults>50){
          this.a=10
      }
      else this.a=Math.ceil(this.imdbResults.totalResults / 5)
      for (let i = 0; i < this.a; i++) {
        this.pages[i] = i + 1;
      }

    },
    pageDown() {
      if (this.pageCount === 1 && this.min === 0) {
      } else if (this.min === 5) {
        this.pageCount -= 1
        this.setMinMaxFirst()
      } else if (this.min === 0) {
        this.pageCount -= 1
        this.changePage()
      }
    },
    pageUp() {
      this.pageCount=parseInt(this.pageCount)
      if (this.min === 0&&this.pageCount !== this.pages.length) {
        this.setMinMaxSecond()
        this.pageCount += 1
      }
      else if (this.min === 5&&this.pageCount !== this.pages.length) {
        this.pageCount += 1
        this.changePage()
      }

    },

    changePage() {


      this.finalUrl = 'https://omdbapi.com/?'
      if (this.pageCount % 2 === 0) {
        this.searchParams.page = Math.floor(this.pageCount / 2)
        this.secondPage = true
      }
      if (this.pageCount % 2 === 1) {
        this.searchParams.page = Math.ceil(this.pageCount / 2)
        this.firstPage = true

      }
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
      this.setMinMaxFirst()
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

    setMinMaxFirst() {
      this.min = 0
      this.max = 5
    },
    setMinMaxSecond() {
      this.min = 5
      this.max = 10
    }
  }
}
</script>
<style scoped>
.pageCounter {
  width: 10vh;
  text-align: center;
}
.form-select{
  font-weight: bold;
  color: whitesmoke;
}

.form-select:hover{
  color: black;
}
.form-select:focus{
  color: black;
}
.pageButtons:hover{
  color: red;
}

</style>
