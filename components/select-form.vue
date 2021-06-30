<template>
  <div >
    <label >{{ selectedItem.title }}</label>
    <v-select
      v-model="selectedItem.value"
      :items="selectedItem.options"
      :label="selectedItem.title"
    >
    </v-select>

  </div>
</template>

<script>
export default {
  name: "selectForm",
  props: {
    nuxtChildKey: {
      type :Object,
      default: ()=> {}
    },
    selectedItem:{

    }

  },

}
</script>

<style scoped>

</style>
