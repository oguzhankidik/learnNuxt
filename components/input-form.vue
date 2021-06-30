<template>
  <div >
    <label>{{selectedItem.title}}</label>
    <v-text-field
      v-model="selectedItem.value"
      :label="selectedItem.title"
      required
    ></v-text-field>
  </div>
</template>

<script>
export default {
  name: "inputForm",
  props: {
    nuxtChildKey: {
      type: Object,
      default: () => {
      }
    },
    selectedItem:{}
  }
}
</script>

<style scoped>


</style>
