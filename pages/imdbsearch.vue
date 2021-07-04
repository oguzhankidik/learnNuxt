<template>

  <div class="pa-6">

    <v-row>

      <v-col

        cols="12"
        sm="12"
        md="3"
      >
        <v-text-field
          outlined
          v-model="searchParams.s"
          label="enter a title"
        ></v-text-field>
      </v-col>
      <v-spacer></v-spacer>
      <v-col
        cols="12"
        offset-md="0"
        sm="12"
        md="3"
      >
        <v-text-field
          outlined
          v-model="searchParams.y"
          label="enter a year"
        ></v-text-field>
      </v-col>
      <v-spacer></v-spacer>
      <v-col
        cols="12"
        sm="12"
        md="3">
        <v-select
          outlined
          v-model="searchParams.type"
          :items="typeOption"
          label="select a type"

        >
        </v-select>
      </v-col>
      <v-col cols="12"
             sm="12"
             md="1"
             offset-md="1">
        <v-btn
          outlined
          block
          rounded
          x-large
          class="pa-4"
          @click="searchClick"
        >Search
        </v-btn>
      </v-col>
    </v-row>


    {{ errorMessage }}
    <imdb-search-card v-if="imdbResults.Response=='True'" :max="max" :min="min" class="mt-4"
                      :selectedItem="imdbResults.Search"/>
    <div v-if="imdbResults.totalResults>5" class="d-flex justify-end align-center mt-4">
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
        ripple="false"
        v-ripple:false
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
      pageCount: {
        type:Number,
        index:1
      },
      firstPage: false,
      secondPage: false,
      pageGenerate: true,
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
      console.log(Math.ceil(34 / 5))
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
      if (this.imdbResults.totalResults > 50) {
        for (let i = 0; i < 10; i++) {
          this.pages[i] = i + 1
        }
      } else {
        let a = Math.ceil(this.imdbResults.totalResults / 5)
        console.log(a)
        for (let i = 0; i < a; i++) {
          this.pages[i] = i + 1;
        }
      }

    },
    pageDown() {
      if (this.pageCount === 1 && this.min === 0) {
        console.log(this.pageCount === 1)
        console.log(this.min === 0)
      } else if (this.min === 5) {
        this.pageCount -= 1
        this.setMinMaxFirst()
      } else if (this.pageCount !== 1 && this.min === 0) {
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
      let abc = {
        s: this.searchParams.s,
        y: this.searchParams.y,
        type: this.searchParams.type,
        page: this.searchParams.page
      }
      let data = new URLSearchParams(abc)
      this.finalUrl += data + this.apiKey
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
      let abc = {
        s: this.searchParams.s,
        y: this.searchParams.y,
        type: this.searchParams.type,
        page: this.searchParams.page
      }
      let data = new URLSearchParams(abc)
      this.finalUrl += data + this.apiKey
      this.getData2(this.finalUrl)
    },
    async getData2(newUrl) {
      const response = await axios.get(newUrl);
      this.imdbResults = response.data

      this.generateErrorMessage()
      this.dropdownPageNumber()
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
