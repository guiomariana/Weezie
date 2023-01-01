<template>
  <div id=map ref="map-root">
  </div>
  <div id="popup" class="ol-popup">
    <a href="#" id="popup-closer" class="ol-popup-closer"></a>
    <div id="popup-content"></div>
  </div>
  <div id="oooo" class="ui segment">
        <div class="ui items">
            <div class="item" v-for="place in places">
                <div class="content">
                    <div class="header">{{ place.name }}</div>
                    <div class="meta">{{ place.rating }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
  #map{
    background: green;
    float: right;
    width: 75%; 
    height: 100%
  }
  #oooo{
    float: left;
    text-align: left;
    width: 25%; 
    height: 100%;
  }
</style>

<script>
  import View from 'ol/View'
  import Map from 'ol/Map'
  import TileLayer from 'ol/layer/Tile'
  import VectorLayer from 'ol/layer/Vector'
  import VectorSource from 'ol/source/Vector'
  import Feature from 'ol/Feature'
  import OSM from 'ol/source/OSM'
  import { Point } from 'ol/geom'
  import { fromLonLat } from 'ol/proj'
  import 'ol/ol.css'
  import axios from "axios";

  export default {
    name: 'MapContainer',
    components: {},
    props: {},

    data(){
      return {
        places: []
      }
    },

    mounted() {
      const URL = `https://cors-anywhere.herokuapp.com/https://maps.googleapis.com/maps/api/place/textsearch/json?query=porto+point+of+interest&language=en&key=AIzaSyDkbf5MsXZ7eUqxFouSc0yylfOAjOHfgi4`;
     
      axios.get(URL).then(response => {
        console.log(response.data);
        this.places = response.data.results;
      }).catch(error => {
        console.log(error.message);
      });

      this.places.forEach(element => console.log(element.geometry.location.lng));
      this.places.forEach(element => console.log(element.business_status));

      new Map({
        target: this.$refs['map-root'],
        layers: [
          new TileLayer({
            source: new OSM() 
          }),

          new VectorLayer({
            source: new VectorSource({
              features: [
                new Feature({
                  geometry: 
                    this.places.forEach(element => new Point(fromLonLat([element.geometry.location.lng, element.geometry.location.lat]))),
                })
              ]
            })
          })
        ],
        
        view: new View({
          zoom: 0,
          center: [0, 0],
          constrainResolution: true
        }),
      })
    },
  }
</script>