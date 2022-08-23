<template>
  <l-map style="height: 300px" :zoom="zoom" :center="center">
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
        <l-marker :lat-lng="markerLatLng"></l-marker>
      </l-map>

</template>
 <script>
import "leaflet/dist/leaflet.css";
import L from "leaflet";

import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import "leaflet/dist/leaflet.css";

export default {
 name: "MapBlock",
 props: ['location'],
 components(){
       LMap,
    LTileLayer,
    LMarker
 },
 data() {
   return{
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
      center: this.location,
      markerLatLng: this.location,
   }},
 methods: {
   setupLeafletMap: function () {
     const mapDiv = L.map("mapContainer").setView(this.center, 13);
     L.tileLayer(
       "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}",
       {
         attribution:
           'Map data (c) <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
         maxZoom: 18,
         id: "mapbox/streets-v11",
         accessToken:'XXX',
       }
     ).addTo(mapDiv);
   },
 },

 mounted() {
   this.setupLeafletMap();
 },
};
</script>

<style scoped>
#mapContainer {
 width: 80vw;
 height: 100vh;
}