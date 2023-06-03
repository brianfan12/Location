<template>
    <div>
      <button class="btn btn-primary" @click="getLocation">Get Current Location</button>
    </div>
</template>
  
<script>
  export default {
    methods: {
      getLocation() {
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(
            (position) =>{
              const lat = position.coords.latitude;
              const lng = position.coords.longitude;
              // emit event to parent component with the coordinates
              this.$emit("location-retrieved", { lat, lng });
            },
            (error) => {
              console.log(error);
            }
          );
        } else {
          console.log("Geolocation is not supported by this browser.");
        }
      },
    },
  };
</script>