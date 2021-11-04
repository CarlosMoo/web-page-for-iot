<template>
  <div class="container">
    <div class="column is-8-desktop">
      <div class="card">
  
        <div class="card-content">
          <div class="media">
            <div class="media-left">
              <figure class="image is-48x48">
                <img src="https://bulma.io/images/placeholders/96x96.png" alt="Placeholder image">
              </figure>
            </div>
            <div class="media-content">
              <p class="title is-4">John Smith</p>
              <p class="subtitle is-6">@johnsmith</p>
            </div>
          </div>

    <div class="content">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      Phasellus nec iaculis mauris. <a>@bulmaio</a>.
      <a href="#">#css</a> <a href="#">#responsive</a>
      <br>
      <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
      <LineChart
          :chartData="arrData"
          :options="chartOptions"
          :chartColors="recoveredColors"
          label="Temperatura"
        />
      <b-button
        expanded
        class="is-white-reaction"
        label="Promedio"
        type="is-light"
        @click="promedium"
      />
      <b-button
        expanded
        class="is-white-reaction"
        label="Maximo"
        type="is-light"
        @click="maximum"
      />
      <b-button
        expanded
        class="is-white-reaction"
        label="Minimo"
        type="is-light"
        @click="minimum"
      />
      <b-button
        expanded
        class="is-white-reaction"
        label="Extraer"
        type="is-light"
        @click="extraer"
      />
    </div>
  </div>
</div>
    </div>
    <div class="column is-4-desktop">
      <div class="card">
  
        <div class="card-content">
          <div class="media">
            <div class="media-left">
              <figure class="image is-48x48">
                <img src="https://bulma.io/images/placeholders/96x96.png" alt="Placeholder image">
              </figure>
            </div>
            <div class="media-content">
              <p class="title is-4">John Smith</p>
              <p class="subtitle is-6">@johnsmith</p>
            </div>
          </div>

    <div class="content">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit.
      Phasellus nec iaculis mauris. <a>@bulmaio</a>.
      <a href="#">#css</a> <a href="#">#responsive</a>
      <br>
      <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
      <LineChart
          :chartData="arrData"
          :options="chartOptions"
          label="Temperatura"
        />
      <b-button
        expanded
        class="is-white-reaction"
        label="Promedio"
        type="is-light"
        @click="promedium"
      />
      <b-button
        expanded
        class="is-white-reaction"
        label="Maximo"
        type="is-light"
        @click="maximum"
      />
      <b-button
        expanded
        class="is-white-reaction"
        label="Minimo"
        type="is-light"
        @click="minimum"
      />
      <b-button
        expanded
        class="is-white-reaction"
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
