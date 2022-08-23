<template>
  <div>
<hr>
    <p v-html="info.query.search[0].snippet"></p>
    <a :href="'https://es.wikipedia.org/?curid='+info.query.search[0].pageid">Ver más </a>
    </div>
</template>

<script>
export default {
  name: "WikiData",
  props: ["nameCity"],
  data() {
    return {
      info: "",
      infoMore: "",
    };
  },
  created() {
    
    this.giveInfo(this.nameCity);
  },
  methods: {
    giveInfo(nameCity) {
      const axios = require("axios");
      var vm = this;
      const options = {
        method: "GET",
        url:
          "https://es.wikipedia.org/w/api.php?action=query&list=search&srprop=snippet&format=json&origin=*&utf8=&srsearch=" +
          nameCity,
      };

      axios
        .request(options)
        .then(function (response) {
          vm.info = response.data;
          vm.giveInfoDetails(vm.info.query.search[0].pageid);
          console.log(response.data);
        })
        .catch(function () {});
    },
    giveInfoDetails(id) {
      const axios = require("axios");
      var vm = this;
      const options = {
          
        method: "GET",
       headers: {
'Access-Control-Allow-Origin':'*'
 },
        url: `https://en.wikipedia.org/w/api.php?action=query&prop=info&pageids=${id}&inprop=url`,
      };

      axios
        .request(options)
        .then(function (response) {
          vm.infoMore = response.data;
          console.log(response.data);
        })
        .catch(function () {});
    },
  },
};
</script>
