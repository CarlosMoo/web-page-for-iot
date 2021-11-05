<template>
  <div class="container">
    <div class="column is-6-desktop">
      <div class="card">
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">Temperatura</p>
              <p class="subtitle is-6">Consulte las últimas mediciones</p>
            </div>
          </div>
          <div class="content">
            <LineChart
                :chartData="arrData"
                :options="chartOptions"
                :chartColors="recoveredColors"
                label="Temperatura"
              />
            <b-button
              expanded
              class="is-success is-rounded"
              label="Promedio"
              type="is-light"
              @click="promedium"/>
            
            <p style="margin-top: 20px; font-weight: bold">{{promedio}}</p>
            <b-button
              expanded
              class="is-info is-rounded"
              label="Maximo"
              type="is-light"
              @click="maximum"
            />
            <p style="margin-top: 20px; font-weight: bold">{{maximo}}</p>
            <b-button
              expanded
              class="is-link is-rounded"
              label="Minimo"
              type="is-light"
              @click="minimum"
            />
            <p style="margin-top: 20px; font-weight: bold">{{minimo}}</p>
            <b-button
              expanded
              class="is-warning is-rounded"
              label="Extraer"
              type="is-light"
              @click="extraer"
            />
          </div>
        </div>
      </div>
    </div>

    <div class="column is-6-desktop">
      <div class="card">
  
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">Humedad</p>
              <p class="subtitle is-6">Consulte las últimas mediciones</p>
            </div>
          </div>

    <div class="content">
      <LineChart2
          :chartData="arrData2"
          :options="chartOptions"
          :chartColors="recoveredColors2"
          label="Humedad"
        />
      <b-button
        expanded
        class="is-success is-rounded"
        label="Promedio"
        type="is-light"
        @click="promediumHum"
      />
      <p style="margin-top: 20px; font-weight: bold">{{promedioHum}}</p>
      <b-button
        expanded
        class="is-info is-rounded"
        label="Maximo"
        type="is-light"
        @click="maximumHum"
      />
      <p style="margin-top: 20px; font-weight: bold">{{maximoHum}}</p>
      <b-button
        expanded
        class="is-link is-rounded"
        label="Minimo"
        type="is-light"
        @click="minimumHum"
      />
      <p style="margin-top: 20px; font-weight: bold">{{minimoHum}}</p>
      <b-button
        expanded
        class="is-warning is-rounded"
        label="Extraer"
        type="is-light"
        @click="extraer"
      />
    </div>
  </div>
</div>
    </div>
    <h1>{{result.channel.name}}</h1>
    <h1>{{result.feeds[0].field1}}</h1>
    <h1>{{promedio}}</h1>
    <p>{{result}}</p>
    <p>{{maximo}}</p>
    <p>{{minimo}}</p>
    <LineChart2
          :chartData="arrData2"
          :options="chartOptions"
          label="Humedad"
        />
</div>
</template>

<script>
// @ is an alias to /src

import axios from 'axios';
import moment from "moment";
import LineChart from "@/components/LineChart.vue";
import LineChart2 from "@/components/LineChart2.vue";
export default {
  name: 'Home',
  data: () => ({
    result: [],
    promedio: 0,
    maximo: 0,
    minimo: 0,
    promedioHum: 0,
    maximoHum: 0,
    minimoHum: 0,
    arrData: [],
    arrData2: [],
    chartOptions: {
      responsive: true,
      maintainAspectRatio: false
    },
    recoveredColors: {
        borderColor: "#4E5E66",
        pointBorderColor: "#4E5E66",
        pointBackgroundColor: "#31E981",
        backgroundColor: "#31E981"
      },

    recoveredColors2: {
    borderColor: "#4E5E66",
    pointBorderColor: "#4E5E66",
    pointBackgroundColor: "#c237db",
    backgroundColor: "#c237db"
      },
  }),
  components: {
    LineChart,
    LineChart2
  },
  methods: {
    promedium() {
      this.result.feeds.forEach(el => {
        this.promedio += parseFloat(el.field1)/parseFloat(this.result.channel.last_entry_id)
      })
      this.promedio = this.promedio.toFixed(2)
    },
    maximum() {
      var arr1 = []
      this.result.feeds.forEach(el => {
        arr1.push(parseFloat(el.field1))
      });
      this.maximo = Math.max(...arr1)
    },
    minimum() {
      var arr2 = []
      this.result.feeds.forEach(el => {
        arr2.push(parseFloat(el.field1))
      });
      this.minimo = Math.min(...arr2)
    },
    extraer() {
      var arr3 = []
      this.result.feeds.forEach(el => {
        arr3.push(el.created_at.substring(0, 10))
      });
      console.log(arr3);
    },
    promediumHum() {
      this.result.feeds.forEach(el => {
        this.promedioHum += parseFloat(el.field2)/parseFloat(this.result.channel.last_entry_id)
      })
      this.promedioHum = this.promedioHum.toFixed(2)
    },
    maximumHum() {
      var arr1 = []
      this.result.feeds.forEach(el => {
        arr1.push(parseFloat(el.field2))
      });
      this.maximoHum = Math.max(...arr1)
    },
    minimumHum() {
      var arr2 = []
      this.result.feeds.forEach(el => {
        arr2.push(parseFloat(el.field2))
      });
      this.minimoHum = Math.min(...arr2)
    },
  },
  async mounted() {
    await axios.get("https://api.thingspeak.com/channels/1534154/feeds.json").then((result) => {
      this.result = result.data;
    })
    this.result.feeds.forEach(el => {
      const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
      //const date = el.entry_id
      this.arrData.push({date, total: parseFloat(el.field1)})
      this.arrData2.push({date, total: parseFloat(el.field2)})
    });
    console.log(this.arrData);
  },
};

</script>
