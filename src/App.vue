<script>
  import LocationButton from './components/LocationButton.vue';
  import LocationSearch from './components/LocationSearch.vue';
  import LocationMap from './components/LocationMap.vue';
  import LocationTable from './components/LocationTable.vue';
  import LatestLocationTime from './components/LatestLocationTime.vue';
  export default {
    components: {
    LocationButton,
    LocationSearch,
    LocationMap,
    LocationTable,
    LatestLocationTime
  },
  data() {
    return{
      center: {
        lat: 37.7749,
        lng: -122.4194 ,
      },
      RMcenter: {
        lat: 37.7749,
        lng: -122.4194 ,
      },
      LocationName: "",
      places: [],
    }
  },
  methods: {
    onLocationRetrieved(coords) {
      this.center = coords;
    },
    onLocationFound(coords){
      this.center = coords;
    },
    onLocationName(name){
      this.LocationName = name;
    },
    onRemoveMarker(coords){
      this.RMcenter = coords;
    }
  },
  watch: {
    LocationName(newLocationName) {
      this.places.push({
        id: this.places.length + 1,
        LocationName: newLocationName,
        center: this.center
      })
    }
  }
}
</script>

<template>
  
  <div>

    <LocationButton @location-retrieved="onLocationRetrieved" />
    <br>
    <locationSearch @location-found="onLocationFound"  @location-name="onLocationName"/>
    <br>
    <LocationMap  :places="places" :center="center" :RMcenter="RMcenter"/>
    <br>
    <LocationTable @remove-marker="onRemoveMarker" :places="places"/>
    <br>
    <LatestLocationTime :center="center" />

  </div>

</template>


