<template>
  <v-row class="justify-center">
    <v-col cols="2" v-for="item in 5" :key="item" class="pa-0">
      <v-card flat dark class="transparent" height="162">
        <v-card-title class="justify-center text-h6 px-0">{{
          data?.day[item - 1]
        }}</v-card-title>
        <v-img
          :src="data?.icon[item - 1]"
          lazy-src="http://openweathermap.org/img/wn/04d.png"
          height="100"
          width="100"
          class="mx-auto my-n7"
        ></v-img>
        <v-card-text class="text-center">
          {{ data?.desc[item - 1] }}
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";
export default {
  name: "my-forecast-page",
  data() {
    return {
      linK: "https://api.openweathermap.org/data/2.5/forecast?q=klaten&appid={API Key}",
      daysName: [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ],
      timestamp: [],
      data: {
        icon: [],
        day: [],
        desc: [],
      },
    };
  },
  methods: {},
  beforeMount() {
    axios.get(this.linK).then((response) => {
      for (var i = 0; i < response.data.list.length; i++) {
        var data = response.data.list[i].dt_txt;
        if (data.includes("00:00:00")) {
          var icon =
            "http://openweathermap.org/img/wn/" +
            response.data.list[i].weather[0].icon +
            "@4x.png";
          this.data.icon.push(icon);
          var day = response.data.list[i].dt_txt;
          var nD = new Date(day);
          this.data.day.push(this.daysName[nD.getDay()]);
          var desc = response.data.list[i].weather[0].description;
          this.data.desc.push(desc);
        }
      }
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