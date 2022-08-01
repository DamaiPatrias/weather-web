<template>
  <v-img :height="windowHeight" src="https://picsum.photos/id/1045/1280/720">
    <v-container fluid fill-height align-end>
      <v-row class="justify-center">
        <v-col>
          <v-card flat class="transparent">
            <v-card-text class="text-center justify-center">
              <div class="text-h2">{{ data.negara }}</div>
              <v-img
                :src="icon"
                height="200"
                width="200"
                class="my-n8 mx-auto"
              ></v-img>
              <div class="font-weight-black text-h5 text--primary">
                {{ data.deskr }}
              </div>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
      <v-row class="background-transparent">
        <v-col cols="6" class="pa-0">
          <v-btn
            tile
            elevation="0"
            dark
            block
            text
            class="py-5"
            v-bind:class="{ activeButton: current }"
            v-on:click="currentMethod"
            >Current</v-btn
          >
        </v-col>
        <v-col cols="6" class="pa-0">
          <v-btn
            tile
            elevation="0"
            dark
            block
            text
            class="py-5"
            v-bind:class="{ activeButton: forecast }"
            v-on:click="forecastMethod"
            >Forecast</v-btn
          >
        </v-col>
        <v-col cols="12">
          <my-current v-if="current"></my-current>
          <my-forecast v-if="forecast"></my-forecast>
        </v-col>
      </v-row>
    </v-container>
  </v-img>
</template>

<script>
import axios from "axios";
import MyCurrent from "@/components/MyCurrent.vue";
import MyForecast from "./MyForecast.vue";

export default {
  components: {
    MyCurrent,
    MyForecast,
  },
  data() {
    return {
      windowHeight: 0,
      windowWidth: 0,
      link: "https://api.openweathermap.org/data/2.5/weather?q=klaten&appid={API Key}&lang=id",
      icon: " ",
      data: {
        negara: null,
        deskr: null,
      },
      current: true,
      forecast: false,
    };
  },
  methods: {
    onResize() {
      this.windowHeight = window.innerHeight;
      this.windowWidth = window.innerWidth;
    },

    currentMethod() {
      this.current = true;
      this.forecast = false;
    },

    forecastMethod() {
      this.current = false;
      this.forecast = true;
    },
  },
  mounted() {
    window.addEventListener("resize", this.onResize);
    this.onResize();

    axios.get(this.link).then((respose) => {
      this.icon =
        "http://openweathermap.org/img/wn/" +
        respose.data.weather[0].icon +
        "@4x.png";
      this.data.negara = respose.data.name;
      this.data.deskr = respose.data.weather[0].description;
    });
  },
  // destroyed() {
  //   window.removeEventListener("resize", this.onResize);
  // },
};
</script>

<style scoped>
.background-transparent {
  background-color: rgba(0, 0, 0, 0.4) !important;
  backdrop-filter: blur(6px);
}

.activeButton {
  border-bottom: 2px solid rgba(255, 255, 255, 0.5) !important;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>