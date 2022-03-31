<template>
  <div>
    <b-container class="pt-4">
      <b-card
        header-bg-variant="secondary"
        border-variant="secondary"
        header-border-variant="secondary"
        bg-variant="success"
      >
        <template #header>
          <h2 class="mb-0 text-center text-light">WeatherApp</h2></template
        >
        <b-card-body>
          <b-row>
            <b-col>
              <b-form-input
                placeholder="Szukaj Miasta"
                v-model="zapytanie"
                type="search"
                @keypress="checkEvent"
                aria-describedby="input-1"
                :state="this.$data.stan"
              ></b-form-input
              ><b-form-invalid-feedback id="input-1"
                >Niepoprawna nazwa miasta</b-form-invalid-feedback
              ></b-col
            >
          </b-row>
          <b-row class="pt-2">
            <b-col
              cols="12"
              lg="4"
              md="6"
              class="pt-4"
              v-for="(miejsc, id) in data"
              :key="id"
            >
              <div v-if="typeof miejsc.main != 'undefined'" align="center">
                <b-button
                  title="Usuń"
                  class="text-right"
                  variant="transparent"
                  @click="removeCity(id)"
                >
                  <b-icon icon="x-circle" variant="danger"></b-icon
                ></b-button>
                <b-card
                  align="center"
                  header-bg-variant="primary"
                  border-variant="secondary"
                  header-border-variant="secondary"
                >
                  <template #header>
                    <h2 class="mb-0 text-light">{{ miejsc.name }}</h2>
                  </template>
                  <b-card-text>
                    <h3>
                      <b-img :src="imgUrl(miejsc.weather[0].icon)"></b-img>
                      {{ round(miejsc.main.temp) }}°C
                    </h3></b-card-text
                  >
                  <b-card-text>
                    Odczuwalne: {{ round(miejsc.main.feels_like) }}°C
                  </b-card-text>
                  <b-card-text>Wiatr: {{ miejsc.wind.speed }}m/s</b-card-text>
                  <b-card-text
                    ><b-icon icon="droplet-half"></b-icon>
                    {{ miejsc.main.humidity }}%</b-card-text
                  >
                  <b-button @click="modal = !modal">
                    <b-icon icon="cloud"></b-icon
                  ></b-button>
                  <b-modal v-model="modal"
                    ><BarView :miasto="miejsc.name" :kraj="miejsc.sys.country"
                  /></b-modal>
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
import BarView from "@/views/BarView.vue";

@Component({
  components: {
    BarView,
  },
})
export default class HomeView extends Vue {
  data() {
    return {
      url: "https://api.openweathermap.org/data/2.5/",
      zapytanie: "",
      pogoda: {},
      data: [],
      stan: true,
      polling: null,
      miejscowosci: [],
      number: 0,
      modal: false,
    };
  }
  created() {
    this.refreshData();
    this.$data.polling = setInterval(this.refreshData, 60000);
  }
  getWeather(refresh: boolean) {
    fetch(
      `${this.$data.url}weather?q=${this.$data.zapytanie}&units=metric&APPID=${this.$store.state.api_key}`
    )
      .then((res) => {
        if (res.status == 200) {
          this.$data.stan = true;
          return res.json();
        } else {
          if (!refresh) {
            this.$data.stan = false;
          }
          throw new Error("Błąd");
        }
      })
      .then((data) => {
        if (!refresh) {
          this.$data.pogoda = data;
          if (typeof this.$data.pogoda.main != undefined) {
            this.$data.data.push(data);
            this.$data.miejscowosci.push(data.name);
          }
        } else if (refresh) {
          if (this.$data.data[this.$data.number] != data) {
            this.$data.data[this.$data.number] = data;
          }
        }
      })
      .catch((error) => {
        console.error("Niepoprawna nazwa miasta");
      });
  }
  checkEvent(e: KeyboardEvent) {
    if (e.key == "Enter") {
      this.getWeather(false);
      this.$data.zapytanie = "";
    }
  }
  async refreshData() {
    const delay = async (ms = 1000) =>
      new Promise((resolve) => setTimeout(resolve, ms));
    for (let i = 0; i < this.$data.miejscowosci.length; i++) {
      this.$data.zapytanie = this.$data.miejscowosci[i];
      this.$data.number = i;
      this.getWeather(true);
      console.log(this.$data.data[this.$data.number].main.temp);
      this.$data.zapytanie = "";
      await delay(1000);
    }
  }
  removeCity(index: any) {
    this.$data.data.splice(index, 1);
    this.$data.miejscowosci.splice(index, 1);
  }
  round(value: number) {
    var mult = Math.pow(10, 1 || 0);
    return Math.round(value * mult) / mult;
  }
  imgUrl(value: string) {
    return "http://openweathermap.org/img/wn/" + value + "@2x.png";
  }
  beforeDestroy() {
    console.log("destroy");
    clearInterval(this.$data.polling);
  }
  chart() {
    this.$router.push({ name: "chart", params: { data: this.$data.data } });
  }
}
</script>

<style scoped lang="scss">
@import "@/assets/scss/vendors/bootstrap-vue/_custom.scss";
@import "~bootstrap/scss/bootstrap.scss";
@import "~bootstrap-vue/dist/bootstrap-vue.css";

h2 {
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
}
</style>
