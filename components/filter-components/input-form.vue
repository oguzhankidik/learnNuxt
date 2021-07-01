<template>
  <div >
    <label>{{selectedItem.title}}</label>
    <v-text-field
      v-model="selectedItem.value"
      :label="selectedItem.title"
    ></v-text-field>
  </div>
</template>

<script>
export default {
  name: "inputForm",

  props: {
    selectedItem:{
      type :Object,
      default: ()=> {}
    }
  }
}
</script>

<style scoped>


</style>
