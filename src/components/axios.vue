<template>
  <div class="hello">
    <!-- (a) -->
    <h1>{{ msg }}</h1>
    <div class="row">
      <!-- (b) -->
      <div
        class="col-sm-6 col-md-3 col-xl-2"
        v-for="(result, idx) in processedResults"
        :key="result.id"
      >
        <!-- (c) -->
        <a class="card" :href="result.post_url">
          <!-- (d) -->
          <h5 class="card-title">{{idx}}:{{result.author}}</h5>
          <!-- (e) -->
          <img class="card-img" :src="result.url" :alt="result.post_url">
          <p class="card-text">{{result.filename}}.</p>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      info: null,
      results: []
    };
  },
  // (f) 
  mounted: function() {
    {
      axios
        .get("https://api.coindesk.com/v1/bpi/currentprice.json")
        .then(response => (this.info = response.data.bpi));

      axios
        .get("https://picsum.photos/list")
        .then(response => (this.results = response.data));
    }
  },
  computed: {
    // (g) 
    processedResults: function() {s
      let results = this.results;
      for (let i = 0; i < results.length; i++) {
        results[i].url = "https://picsum.photos/300/200?image=" + results[i].id;
      }
      return results;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  color: #42b983;
}
</style>