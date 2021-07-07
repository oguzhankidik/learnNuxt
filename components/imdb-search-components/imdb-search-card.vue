<template>
  <div  class="row col-10">

    <div v-for="(item,index) in selectedItem" :key="index" class="card col-3" >
      <NuxtLink class="nuxtlink" :to="{name:'id-details' , params: {id:item.imdbID}}" >
      <v-img class="card-img-top mb-2" :src="item.Poster" width="280" height="400" alt="Poster can't be found."> </v-img>
      <div class="card-body">
        <h5 class="card-title">{{ item.Title }}</h5>
        <p class="card-text">
          Release Date : {{ item.Year }}<br>
          Type : {{ item.Type }}
        </p>
      </div>
      </NuxtLink>
    </div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "selectedItem",
  data() {
    return {
      apiKey: '&apikey=6448bbcf',
      url: 'http://www.omdbapi.com/?'
    }
  },
  props: {
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
  max-width: 300px;
  word-wrap: break-word;
  overflow: hidden;
  max-height: 1.8em;
  line-height: 1.8em;
}
.card-img-top{
  border: 1px solid grey;
}
.nuxtlink{
  text-decoration: none;
  color: inherit;
}
</style>
