<template>
  <div>
    <b-container>
      <b-card>
        <b-card-title class="text-center">WeatherApp</b-card-title>
        <b-card-body>
          <b-row>
            <b-col>
              <b-form-input
                class="mr-sm-2"
                placeholder="Szukaj"
                v-model="zapytanie"
                type="search"
                @keypress="getWeather"
              ></b-form-input
            ></b-col>
          </b-row>
          <b-row class="py-4">
            <b-col cols="4" v-for="(miejsc, id) in miejscowosci" :key="id">
              <div v-if="typeof miejsc.name != 'undefined'">
                <b-card :header="miejsc.name">
                  <b-card-text> {{ miejsc.main.temp }}°C </b-card-text>
                </b-card>
              </div>
            </b-col>
          </b-row>
        </b-card-body>
      </b-card>
    </b-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class HomeView extends Vue {
  data() {
    return {
      url: "https://api.openweathermap.org/data/2.5/",
      zapytanie: "",
      pogoda: {},
      miejscowosci: [],
    };
  }
  getWeather(e: KeyboardEvent) {
    if (e.key == "Enter") {
      fetch(
        `${this.$data.url}weather?q=${this.$data.zapytanie}&units=metric&APPID=${this.$store.state.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    }
  }
  setResults(data: string) {
    this.$data.pogoda = data;
    if (typeof this.$data.pogoda.main != undefined) {
      this.$data.miejscowosci.push(data);
    }
  }
}
</script>

<style scoped></style>
