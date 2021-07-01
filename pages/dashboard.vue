<template>
  <v-card>
    <dashboard-chart v-if="chartLoaded" v-bind:message="posts.online_guests" />
    <dashboard-info v-bind:message2="this.posts" :maxvalue="maxVal" />
    <div class="row">
      <dashboard-pie-chart class="col-lg-6 col-sm-6 col-12" v-if="chartLoaded" v-bind:message3="posts.device_vendors" />
      <dashboard-pie-chart class="col-lg-6" v-if="chartLoaded" v-bind:message3="posts.login_types" />
    </div>
  </v-card>
</template>

<script>
import dashboardInfo from "~/components/dashboard-components/dashboard-info";
import dashboardChart from "~/components/dashboard-components/dashboard-chart";
import axios from "axios";
import dashboardPieChart from "~/components/dashboard-components/dashboard-piechart";
export default {
  components: {
    dashboardInfo, dashboardChart,dashboardPieChart,
  },
  name: 'Dashboard',
  props: {
    msg: String
  },
  created() {
    this.getPosts()
  },
  data() {
    return {
      chartLoaded:false,
      maxVal: '',
      posts: [],
      errors: []
    };
  },
  methods: {
    async getPosts() {
      const response=await axios
        .get("http://challenge.iperasolutions.com/dashboard");
      this.posts=response.data
      this.maxVal = Math.max.apply(Math, this.posts.online_guests);
      this.chartLoaded=true;
    },
  }
}
</script>


<style scoped>
.bottominfos{
  display: flex;
  padding: 20px;
  justify-content: space-between;
}
</style>
