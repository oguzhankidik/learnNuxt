<template>
  <div class="pa-6">
    <v-row>
      <v-col class="col-sm-12 col-lg-4 col-md-6 col-12">
    <v-text-field
      outlined
      v-model="searchParams.s"
      label="enter a title"
    ></v-text-field>
      </v-col>
      <v-col class="col-sm-12 col-lg-4 col-md-6 col-12">
    <v-text-field
      outlined
      v-model="searchParams.y"
      label="enter a year"
    ></v-text-field>
      </v-col>
      <v-col class="col-sm-12 col-lg-4 col-md-6 col-12">
    <v-select
      outlined
      v-model="searchParams.type"
      :items="typeOption"
      label="select a type"
    >
    </v-select>
      </v-col>
    </v-row>
    <v-btn
      class="d-flex justify-content-end"
      rounded
      @click="clickSearch"
    >Search
    </v-btn>


    <div  class="row mt-4">
      {{errorMessage}}
      <div v-for="(item,index) in imdbResults.Search" class="col" :key="index">
        <imdb-search-card :selectedItem="item"/>
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
      imdbResults: [],
      apiKey: '&apikey=6448bbcf',
      typeOption: [
        "movie", "series", "short"
      ],
      errorMessage:'',
      searchParams: { 's': '', 'y': '', 'type': '' },
      ret:[],
      finalUrl:'https://omdbapi.com/?'
    }
  },

  methods: {
    async getData(newUrl) {
      const response = await axios.get(newUrl);
      this.imdbResults = response.data
      this.generateErrorMessage()
    },

    clickSearch() {

      this.ret=[]
      this.finalUrl='https://omdbapi.com/?'
      for (let d in this.searchParams)
        this.ret.push(encodeURIComponent(d) + '=' + encodeURIComponent(this.searchParams[d]));
      this.finalUrl+=this.ret.join('&')+this.apiKey
      this.getData(this.finalUrl)


    },
    generateErrorMessage(){
      if(this.imdbResults.Error==="Incorrect IMDb ID."){
        this.errorMessage='Please Enter a Title'
      }
      else{this.errorMessage=this.imdbResults.Error}
    }
  }
}
</script>
