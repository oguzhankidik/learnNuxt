<template>
  <div  class="row">
    <div v-for="(item,index) in selectedItem.slice(this.min,this.max)" :key="index" class="card col" >
      <v-img class="card-img-top mb-2" :src="item.Poster" width="300" height="400" alt="Poster can't be found."> </v-img>
      <div class="card-body">
        <h5 class="card-title">{{ item.Title }}</h5>
        <p class="card-text">
          Release Date : {{ item.Year }}<br>
          Type : {{ item.Type }}
        </p>
        <v-btn rounded :to="{name:'detail' , params: {id:item.imdbID}}">Details</v-btn>


      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "selectedItem",
  data() {
    return {
      perPage:5,
      pageLimit:5,
      apiKey: '&apikey=6448bbcf',
      url: 'http://www.omdbapi.com/?'
    }
  },
  props: {
    min:{
      type:Number
    },
    max:{
      type:Number
    },
    selectedItem: {
      type: Array,
      default: () => {
      },
      required:true
    }
  },
  methods: {

  }
}
</script>

<style scoped>

.card-img-top{
  border-radius: 10px;
}
.card:hover{
  transform: scale(1.05);

}
.card-title{

  word-wrap: break-word;
  overflow: hidden;
  max-height: 1.8em;
  line-height: 1.8em;
}

</style>
