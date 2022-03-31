<template>
  <b-container fluid>
    <VueApexCharts
      type="line"
      witdh="100%"
      :options="options"
      :series="series"
    ></VueApexCharts
    ><VueApexCharts
      type="line"
      :options="options2"
      :series="series2"
    ></VueApexCharts>
  </b-container>
</template>
<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import VueApexCharts from "vue-apexcharts";

@Component({
  components: {
    VueApexCharts,
  },
})
export default class BarView extends Vue {
  @Prop({ default: "" }) readonly miasto!: string;
  @Prop({ default: "" }) readonly kraj!: string;
  created() {
    this.getHistoryWeather();
  }

  data() {
    return {
      url: "http://history.openweathermap.org/data/2.5/history/city?q=",
      options: {
        chart: { id: "vuechart" },
        xaxis: {
          title: {
            text: "Czas",
          },
          labels: {
            formatter: (val: number, index: number) => {
              const d = new Date();
              val = val - d.getUTCHours();
              if (val < 0) {
                val = val + 24;
              }
              return val.toFixed(0);
            },
          },
        },
        title: {
          text: "Temperatura",
          align: "left",
        },
      },
      options2: {
        chart: { id: "vuechart2" },
        xaxis: {
          title: {
            text: "Czas",
          },
          labels: {
            formatter: (val: number, index: number) => {
              const d = new Date();
              val = val - d.getUTCHours();
              if (val < 0) {
                val = val + 24;
              }
              return val.toFixed(0);
            },
          },
        },
        title: {
          text: "Wilgotność",
          align: "left",
        },
      },

      series: [
        {
          name: "Temperatura",
          data: [],
        },
      ],
      series2: [
        {
          name: "Wilgotnosc",
          data: [],
        },
      ],
    };
  }
  getHistoryWeather() {
    const parsed = this.miasto.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
    fetch(
      `${this.$data.url}${parsed},${this.kraj}&units=metric&APPID=${this.$store.state.api_key}`
    )
      .then((res) => {
        if (res.status == 200) {
          return res.json();
        } else {
          throw new Error("Błąd");
        }
      })
      .then((data) => {
        const newData = data.list.map((item: any) => {
          return this.round(item.main.temp);
        });
        const newData2 = data.list.map((item: any) => {
          return this.round(item.main.humidity);
        });
        this.$data.series = [
          {
            data: newData,
          },
        ];
        this.$data.series2 = [
          {
            data: newData2,
          },
        ];
      });
  }
  round(value: number) {
    var mult = Math.pow(10, 1 || 0);
    return Math.round(value * mult) / mult;
  }
}
</script>
