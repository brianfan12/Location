<template>
  <div>
    <div>Time Zone: {{ timeZone }}</div>
    <div>Local Time: {{ localTime }}</div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
import 'moment-timezone';

export default {
  props: {
    center: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      timeZone: null,
      localTime: null,
      formattedLocalTime: ''
    }
  },
  watch: {
    center: {
      immediate: true,
      handler() {
        this.fetchTimeZone();
      }
    },
    timeZone(newTimeZone){
      let now = new Date();
    this.localTime = now.toLocaleString('en-US', { timeZone: newTimeZone });
    }
  },
  methods: {
    async fetchTimeZone() {
      const { lat, lng } = this.center;
      const timestamp = Math.floor(Date.now() / 1000);
      const apiKey = 'AIzaSyDyfm_-ncheN1xpa0F12TCYLYdf_L3fPLE';
      const apiUrl = `https://maps.googleapis.com/maps/api/timezone/json?location=${lat},${lng}&timestamp=${timestamp}&key=${apiKey}`;

      try {
        const response = await axios.get(apiUrl);
        const { timeZoneId, dstOffset, rawOffset } = response.data;
        //const localTime = new Date(Date.now() + (dstOffset + rawOffset) * 1000);
        this.timeZone = timeZoneId;
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>