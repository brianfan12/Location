<template>
  <div>
    <div id="map"  style="height: 500px; width: 500px;" ></div>
  </div>
</template>

<script>
import { Loader } from "@googlemaps/js-api-loader";

export default {
  data() {
    return {
      map: null,
      marker: null,
      //center: { lat: 37.7749, lng: -122.4194 } // San Francisco coordinates
    };
  },
  props: {
     RMcenter: {
      type: Object,
      required: true,
      default: () => ({ lat: 37.7749, lng: -122.4194 })
     },
     places: [],
     center: {
      type: Object,
      required: true,
      default: () => ({ lat: 37.7749, lng: -122.4194 })
    }
  },
  mounted() {
    // Load the Google Maps API
    const loader = new Loader({
      apiKey: "AIzaSyB7q0v-GFIfVlNqy4utxKEfmWAiWANY8aI",
      version: "weekly",
      language: 'en'
    });

    // Initialize the map and marker when the API is ready
    loader.load().then(() => {
      this.map = new google.maps.Map(document.getElementById('map'), {
        center: this.center,
        zoom: 13,
      });

      this.marker = new google.maps.Marker({
        position: this.center,
        map: this.map
      });
    });
  },
  watch: {
    center(newCenter) {
      // Create new markers for each place in the array
      this.places.forEach((place) => {
        const marker = new google.maps.Marker({
          position: place.center,
          map: this.map,
          title: place.LocationName
        });
        place.marker = marker;
      });

      if (this.marker) {
        this.marker.setPosition(newCenter);
      }
      if (this.map) {
        this.map.setCenter(newCenter);
      }
    },
    RMcenter(newRMcenter){
      const loader = new Loader({
      apiKey: "AIzaSyB7q0v-GFIfVlNqy4utxKEfmWAiWANY8aI",
      version: "weekly",
      language: 'en'
    });

    // Initialize the map and marker when the API is ready
    loader.load().then(() => {
      this.map = new google.maps.Map(document.getElementById('map'), {
        center: this.center,
        zoom: 13,
      });
      
      this.places.forEach((place) => {
        const marker = new google.maps.Marker({
          position: place.center,
          map: this.map,
          title: place.LocationName
        });
        place.marker = marker;
      });

      if (this.marker) {
        this.marker.setPosition(this.places[this.places.length-1].center);
      }
      if (this.map) {
        this.map.setCenter(this.places[this.places.length-1].center);
      }
      
      });

    }
  }
};
</script>