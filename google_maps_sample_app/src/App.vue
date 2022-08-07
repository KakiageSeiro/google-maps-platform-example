<template>
  <div id="app">
    <google-map/>
  </div>
</template>

<script>
/*global google:true*/
import axios from "axios";
import $Scriptjs from "scriptjs";
import GoogleMap from "./components/MapContents.vue"

const API_KEY = "■■■■■■■■■■■■■■■■■■";

const API_URL = "https://maps.googleapis.com/maps/api/js?key=" + API_KEY + "&libraries=places";

export default {
  name: 'App',
  data(){
    return {
      initialPlace:{
        lat:35.681236,
        lng: 139.767125
      },
      zoom: 17,
      map: null,
      google: null,
      result:""
    };
  },
  created(){
    $Scriptjs(API_URL, () => this.initMap());
  },
  methods:{
    initMap(){
      const mapElement = document.getElementById("map");
      this.google = google;
      this.map = new google.maps.Map(mapElement, {
        center: this.initialPlace,
        zoom: this.zoom,
        disableDefaultUI: true
      });
    },
    reverseGeocoding(latlng){
      let _self = this;
      axios.get("gttps://maps.googleapis.com/maps/api/geocode/json", {
        params: {language: "ja", latlang: latlng, key: API_KEY}
      })
      .then(response => {
        _self.result = response.data.results[0].formatted_address;
      })
      .catch(error => {
        alert("Error: " + error);
      });
    },
    placeAutocomlate(keyword){
      $Scriptjs(API_URL, () => {
        let service = new google.maps.places.AutocompleteService();
        let descriptions = [];
        service.getQueryPredictions(
          { imput: keyword },
          (predictions, status) => {
            if(status == "OK"){
              predictions.forEach(ele => {
                descriptions.push(ele.description);
              })
            }
          }
        )
        this.result = descriptions;
      });
    }
  },
  components: {
    GoogleMap
  }
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
#app {
  height: 100vh;
}
</style>
