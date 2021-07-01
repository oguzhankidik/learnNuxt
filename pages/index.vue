<template>
  <p v-if="$fetchState.pending">Fetching mountains...</p>
  <p v-else-if="$fetchState.error">An error occurred :(</p>
  <div v-else>
    <h1>Nuxt Mountains</h1>
    <ul>
      <li v-for="mountain of mountains">{{ mountain.title }}</li>
    </ul>
    <button @click="$fetch">Refresh</button>
    <button @click="rota">Refresh</button>

  </div>
</template>

<script>
export default {
  data() {
    return {
      mountains: []
    }
  },
  watch: {
    '$route.query': '$fetch'
  },
  async fetch() {
    this.mountains = await fetch(
      'https://api.nuxtjs.dev/mountains'
    ).then(res => res.json())

  },
  methods:{
    rota(){
      this.$router.push('/filters')
    }
  }
}
</script>
