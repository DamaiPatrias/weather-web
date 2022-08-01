<template>
  <v-row class="justify-center">
    <v-col cols="3">
      <v-card
        flat
        tile
        fill-height
        dark
        class="transparent text-center"
        height="138"
      >
        <v-card-title class="text-h6 justify-center"> SUHU </v-card-title>
        <v-card-text class="font-weight-thin text-h3 px-0">
          {{ Math.round(data.suhu - 273.15) }}&deg;C
        </v-card-text>
      </v-card>
    </v-col>
    <v-col cols="3">
      <v-card flat tile fill-height dark class="transparent text-center">
        <v-card-title class="text-h6 justify-center"> KELEMBABAN </v-card-title>
        <v-card-text class="font-weight-thin text-h3 px-0">
          {{ data.kelembaban }} %
        </v-card-text>
      </v-card>
    </v-col>
    <v-col cols="3">
      <v-card flat tile fill-height dark class="transparent text-center">
        <v-card-title class="text-h6 justify-center"> ANGIN </v-card-title>
        <v-card-text class="font-weight-thin text-h3 px-0">
          {{ data.kecapatanAngin }} m/h
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      windowHeight: 0,
      windowWidth: 0,
      link: "https://api.openweathermap.org/data/2.5/weather?q=klaten&appid={API Key}&lang=id",
      data: {
        suhu: null,
        kelembaban: null,
        kecapatanAngin: null,
      },
    };
  },
  beforeMount() {
    axios.get(this.link).then((respose) => {
      this.data.suhu = respose.data.main.temp;
      this.data.kelembaban = respose.data.main.humidity;
      this.data.kecapatanAngin = respose.data.wind.speed;
    });
  },
};
</script>

<style scoped>
.background-transparent {
  background-color: rgba(0, 0, 0, 0.4) !important;
  backdrop-filter: blur(6px);
}
</style>