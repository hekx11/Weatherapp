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
          <b-row class="pt-4">
            <b-col
              cols="4"
              class="pt-4"
              v-for="(miejsc, id) in miejscowosci"
              :key="id"
            >
              <div v-if="typeof miejsc.main != 'undefined'">
                <b-card align="center">
                  <template #header>
                    <h4 class="mb-0">{{ miejsc.name }}</h4>
                    <b-button
                      title="Usuń"
                      class="text-right"
                      variant="light"
                      @click="removeCity(id)"
                    >
                      <b-icon icon="x-circle" variant="success"></b-icon
                    ></b-button>
                  </template>
                  <b-card-text
                    >Temperatura: {{ miejsc.main.temp }}°C
                  </b-card-text>
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
  getWeather(k: KeyboardEvent) {
    if (k.key == "Enter") {
      fetch(
        `${this.$data.url}weather?q=${this.$data.zapytanie}&units=metric&APPID=${this.$store.state.api_key}`
      )
        .then((res) => {
          if (res.status == 200) {
            this.$data.zapytanie = "";
            return res.json();
          } else {
            this.$data.zapytanie = "";
            throw new Error("Błąd");
          }
        })
        .then(this.setResults)
        .catch((error) => console.error("Niepoprawna nazwa miasta"));
    }
  }
  setResults(data: string) {
    this.$data.pogoda = data;
    if (typeof this.$data.pogoda.main != undefined) {
      this.$data.miejscowosci.push(data);
    }
  }
  removeCity(index: any) {
    this.$data.miejscowosci.splice(index, 1);
  }
}
</script>

<style scoped></style>
