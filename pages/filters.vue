<template>
  <v-card>
    <div class="row">
      <div v-for="(item,index3) in nuxtChildKey.slice(0,arrLimit)" class="col-lg-3" :key="index3">
        <select-form v-if="item.type  === 'select'" :selectedItem="item"/>
        <input-form v-if="item.type  === 'text'" :selectedItem="item"/>
        <date-form v-if="item.type  === 'date'" :selectedItem="item"/>
      </div>


      <div class="d-flex justify-content-end mt-5">
        <v-btn
          depressed
          elevation="9"
          outlined
          rounded
          @click="searchButton"
        >Search</v-btn>

      </div>

      <div >
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
import InputForm from "@/components/input-form";
import SelectForm from "@/components/select-form";

export default {
  name: "filter-page",
  components: {SelectForm, InputForm},
  props: {
    nuxtChildKey: {
      type: Object,
      default: () => {
      }
    },
  },
    data: {
      arrLimit: 5,
      savedFilters: [],
      searchClicked: false,
      isAdvanced: false,
    },

    methods: {
      searchButton() {
        this.savedFilters = JSON.parse(JSON.stringify(this.nuxtChildKey))
        this.searchClicked = true;
      },
    }

}
</script>

<style scoped>

</style>
