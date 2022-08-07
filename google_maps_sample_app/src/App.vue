<template>
  <div id="app">
    <google-map/>
    <div v-if="!!this.google && !!this.map">
      <search-bar
        :map="map"
        :google="google"
        @inputKeyword="placeAutocomplete"
        @mountedSearchBar="mountedSearchBar"
      />
      <result-view
        :map="map"
        :google="google"
        :result="result"
        @mountedResultView="mountedResultView"
      />
    </div>
  </div>
</template>

<script>
/*global google:true*/
import axios from "axios";
import $Scriptjs from "scriptjs";
import GoogleMap from "./components/MapContents.vue";
import SearchBar from './components/SearchBar.vue';
import ResultView from './components/ResultView.vue';

const API_KEY = "■■■■■■■■■■■■";

const API_URL = "https://maps.googleapis.com/maps/api/js?key=" + 
  API_KEY + "&libraries=places" + 
  "&libraries=places";

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
      axios.get("https://maps.googleapis.com/maps/api/geocode/json", {
        params: {language: "ja", latlang: latlng, key: API_KEY}
      })
      .then(response => {
        _self.result = response.data.results[0].formatted_address;
      })
      .catch(error => {
        alert("Error: " + error);
      });
    },
    placeAutocomplete(keyword){
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
    },
    mountedResultView(){
      let resultView = this.$el.querySelector("#result-view");
      this.map.comtroles[this.google.maps.ControlPosition.BOTTOM_CENTER].push(
      resultView
      );
    },
    mountedSearchBar(){
      let searchBox = this.$el.querySelector("#search-bar");
      this.map.comtrols[this.google.maps.ControlPosition.TOP_CENTER].push(
        searchBox
      );
    }
  },
  components: {
    GoogleMap,
    SearchBar,
    ResultView
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
