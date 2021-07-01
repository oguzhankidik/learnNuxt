<template>
<div>
{{mountains}}

  <v-text-field
    v-model="filters.title"
    label="enter title"
  ></v-text-field>
  <v-text-field
    v-model="filters.year"
    label="enter year"
  ></v-text-field>
  <v-btn
  @click="fetch"
  >bas</v-btn>

</div>
</template>


<script>
import axios from "axios";
import filters from "~/pages/filters";

export default {
  data() {
    return {
      posts:[],
      filters:['t','i','t'],
      apiKey:'6448bbcf',
      mountains:[],
      yeniurl:'',
      url:'https://www.omdbapi.com/?apikey=6448bbcf'
    }
  },


  methods:{

    async fetch() {
      for (const item of this.filters) {
        this.yeniurl='https://www.omdbapi.com/?'+item+'=' +'title'+'&apikey='+this.apiKey
        this.mountains = await fetch(
          this.yeniurl
        ).then(res => res.json())
      }


    },
  }
}
</script>
