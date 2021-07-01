<template>
  <div >
    <label >{{ selectedItem.title }}</label>
    <v-select
      v-model="selectedItem.value"
      :items="selectedItem.options"
      :label="this.label"
    >
    </v-select>

  </div>
</template>

<script>
export default {
  name: "selectForm",
  data() {
    return {
      label:"(All "+ this.selectedItem.title+")"
    }
  },
  props: {
    selectedItem:{
      type :Object,
      default: ()=> {}
    }

  },

}
</script>

<style scoped>

</style>
