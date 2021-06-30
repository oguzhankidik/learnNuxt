<template>
  <div>
    <label>{{ selectedItem.title }}</label>
    <input type="date" v-model="selectedItem.value" >
  </div>
</template>

<script>
export default {
  name: "dateForm",
  props: {
    selectedItem: {
      type :Object,
      default: ()=> {}
    },
  },


}
</script>

<style scoped>

</style>
