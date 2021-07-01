<template>
  <v-card
  elevation="14"

  outlined
  class="pa-6"
  >
    <div class="d-flex justify-lg-space-between mb-1">

      <h2>Guests</h2>
      <v-btn
        elevation="4"
        rounded
        @click="advancedSearchButton"
      > Advanced Search
      </v-btn>
    </div>
    <v-divider class="mb-4"/>
    <div class="row">
      <div v-for="(item,index3) in posts.slice(0,arrLimit)" class="col-lg-3" :key="index3">

        <select-form v-if="item.type  === 'select'" :selectedItem="item"/>
        <input-form v-if="item.type  === 'text'" :selectedItem="item"/>
        <date-form v-if="item.type  === 'date'" :selectedItem="item"/>
      </div>

<v-card-actions>
      <div class="d-flex justify-content-end mt-5">
        <v-btn
          depressed
          elevation="9"
          outlined
          rounded
          @click="searchButton"
        >Search
        </v-btn>
        <v-btn
          depressed
          elevation="9"
          outlined
          rounded
          @click="clearButton"
        >Clear
        </v-btn>

      </div>
</v-card-actions>
      <div v-show="searchClicked">
        <ul>
          <li v-for="(item,index) in this.savedFilters" :key="index">
            {{ item.title }} is : {{ item.value }}
          </li>
        </ul>
      </div>
    </div>
  </v-card>
</template>

<script>
import InputForm from "~/components/filter-components/input-form";
import SelectForm from "~/components/filter-components/select-form";
import DateForm from "~/components/filter-components/date-form"
import axios from "axios";

export default {
  name: "filter-page",
  components: {SelectForm, InputForm,DateForm},
  props: {

  },
  created() {
    this.getPosts()
  },

  data() {
    return {
      posts:[],
      arrLimit: 5,
      savedFilters: [],
      searchClicked: false,
      isAdvanced: false,
    }
  },

  methods: {

    async getPosts() {
      let response = await axios.get("http://challenge.iperasolutions.com/filters");
      response.data.forEach(item => {
        if (item.type === 'select') {
          item.value = null
        } else if (item.type === 'text') {
          item.value = ''
        } else {
          item.value = new Date().toISOString().slice(0, 10)
        }
        this.posts.push(item)
      })
      this.dataLoaded = true;
    },
    searchButton() {
      this.savedFilters = JSON.parse(JSON.stringify(this.posts))
      this.searchClicked = true;
    },
    advancedSearchButton() {
      this.arrLimit = this.arrLimit === 5 ? 10 : 5
    },
    clearButton() {
      this.searchClicked = false;
      this.posts.forEach(item => {
        if(item.type === 'select'){
          item.value = null
        } else if( item.type === 'text') {
          item.value = ''
        } else {
          item.value = new Date().toISOString().slice(0,10)
        }
      })
    }
  }

}
</script>

<style scoped>

</style>
