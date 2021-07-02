<template>
  <div class="card" style="width: 18rem;">
    <img class="card-img-top" :src="selectedItem.Poster" alt="Poster can't be found.">
    <div class="card-body">
      <h5 class="card-title">{{ selectedItem.Title }}</h5>
      <p class="card-text">
        Release Date : {{detailedInfo.Released}}<br>
        Director : {{detailedInfo.Director}}<br>
        Actors : {{detailedInfo.Actors}}
      </p>

      <NuxtLink :to="{name:'detail' , params: {id:selectedItem.imdbID}}" >asfsa</NuxtLink>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "selectedItem",
  data() {
    return {
      detailedInfo:[],
      apiKey: '&apikey=6448bbcf',
      url:'http://www.omdbapi.com/?'
    }
  },
  created() {
   this.detailInfo()
  },
  props:{
    selectedItem:{
      type:Object,
      default:()=>{}
    }
  },
  methods:{
    async getData(newUrl) {
      const response = await axios.get(newUrl);
      this.detailedInfo = response.data

    },

    detailInfo(){
      this.url+='i='+this.selectedItem.imdbID+this.apiKey
      this.getData(this.url)
    }
  }
}
</script>

<style scoped>

</style>
