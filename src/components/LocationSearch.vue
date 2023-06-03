<template>
  <div>
    <label for="location-input">Enter a location:</label>
    <input type="text" id="location-input" v-model="locationInput" @keyup.enter="searchLocation" />
    <button @click="searchLocation">Search</button>
  </div>
</template>

<script>
import { Loader } from "@googlemaps/js-api-loader";

export default {
  data() {
    return {
      locationInput: ""
    };
  },
  methods: {
    searchLocation() {
      // Use the Geocoding API to convert the location input into coordinates
      const geocoder = new google.maps.Geocoder();
      geocoder.geocode({ address: this.locationInput }, (results, status) => {
        if (status === "OK") {
          const location = results[0].geometry.location;
          this.$emit("location-found", {
            lat: location.lat(),
            lng: location.lng()
          });
          this.$emit("location-name",this.locationInput);
          this.locationInput = ""; // clear the input field
        } else {
          alert("Sorry, we could not find that location.");
        }
      });
    }
  }
};
</script>