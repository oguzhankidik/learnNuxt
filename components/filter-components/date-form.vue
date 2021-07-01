<template>
  <div>
    <label>{{ selectedItem.title }}</label>
    <div class="mt-4">
    <input type="date" v-model="selectedItem.value">
    </div>
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
.mt-4{
  color: white;
}
</style>
