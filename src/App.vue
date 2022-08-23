<template>
  <div id="app">
    <div v-if="isLoad">
     <h3> Cargando Información</h3>
    <span class="loader"></span>
    </div>
    <div v-else>
    
      <sui-container class="background-ppal">
     <meteo-block :data="data" :error="error"></meteo-block>
      </sui-container>
    </div>
  </div>
</template>

<style>
body{          background-image: url('assets/summer.jpg');
}
</style>


<script>

import MeteoBlock from "@/components/MeteoData"


// import HelloWorld from "./components/HelloWorld.vue";
export default {
  components: {
  
    
    meteoBlock:MeteoBlock,
    
    
  },
  name: "App",

  data() {
    return {
      isLoad: true,
      lat: "",
      long: "",
      api: "google.es",
      data: "",
      error: false,
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 15,
      center: [36.7738869, -6.3545177, 15],
      markerLatLng: [36.7738869, -6.3545177, 15],
    };
  },

  mounted() {


    try {
      this.isLoad = true;
//       var opciones = {
//     enableHighAccuracy: true,
//     timeout: 10000,
//     maximumAge: 1000
// };
      navigator.geolocation.getCurrentPosition(this.showPosition);
    } catch (error) {
      console.error(error);
    
    }
  },
  methods: {
    async showError(error){
switch (error.code) {
        case error.PERMISSION_DENIED:
          this.isLoad=false
            alert('Permiso denegado por el usuario'); 
            break;
        case error.POSITION_UNAVAILABLE:
            alert('Posición no disponible');
            break; 
        case error.TIMEOUT:
            alert('Tiempo de espera agotado');
            break;
        default:
            alert('Error de Geolocalización desconocido :' + error.code);
    }
    },
    showPosition(position) {
      this.lat = position.coords.latitude;
      this.longitude = position.coords.longitude;
      this.center = [this.lat, this.longitude];
      console.log(this.center);
      this.getInfo(position.coords.latitude, position.coords.longitude);
    },
    getInfo(a, b) {
      const axios = require("axios");
      var vm = this;
      const options = {
        method: "GET",
        url: "https://yahoo-weather5.p.rapidapi.com/weather",
        params: {
          lat: a ? a : "36.7738869",
          long: b ? b : "-6.3545177,15z",
          format: "json",
          u: "f",
        },
        headers: {
          "X-RapidAPI-Key": "99833a4266mshe439dc5ecc4747ap155b42jsnab34657115fe",
          "X-RapidAPI-Host": "yahoo-weather5.p.rapidapi.com",
        },
      };

      axios
        .request(options)
        .then(function (response) {
          console.log(response.data);
          vm.data = response.data;
          vm.isLoad = false;
        })
        .catch(function (error) {
          console.error(error);
          this.error = true;
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.loader {
    width: 48px;
    height: 48px;
    border: 5px solid #FFF;
    border-bottom-color: #FF3D00;
    border-radius: 50%;
    display: inline-block;
    box-sizing: border-box;
    animation: rotation 1s linear infinite;
    }

    @keyframes rotation {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
    } 
</style>
