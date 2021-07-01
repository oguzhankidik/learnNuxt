<template>
  <div class="infos">
    <div class="card" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title">Current Online</h5>
        {{ message2.current_online }}
      </div>
    </div>
    <div class="card" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title">Max Online</h5>
        {{maxvalue}}

      </div>
    </div>
    <div class="card" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title">Unique Connections</h5>
        {{message2.unique_connections}}

      </div>
    </div>
    <div class="card" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title">Impressions</h5>
        {{message2.impressions}}

      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Infos',
  props: {
    message2: {
    },
    maxvalue:{

    }
  },

}
</script>

<style scoped>

.infos {
  display: flex;
  padding: 20px;
  align-items: center;
  justify-content: space-between;
}

</style>
