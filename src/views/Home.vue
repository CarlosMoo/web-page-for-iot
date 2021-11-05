<template>
  <div class="container">
    <section class="hero" style="background-color: #233E8B">
      <div class="hero-body">
        <p class="title has-text-white">
          Monitoreo de Humedad y Temperatura
        </p>
      </div>
    </section>
    <div class="columns">
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
              <LineChart :chartData="arrData" :options="chartOptions" :chartColors="recoveredColors"
                label="Temperatura" />
              <b-button expanded class="is-success is-rounded" label="Promedio" type="is-light" @click="promedium" />

              <p style="margin-top: 20px; font-weight: bold">{{promedio}}</p>
              <b-button expanded class="is-info is-rounded" label="Maximo" type="is-light" @click="maximum" />
              <p style="margin-top: 20px; font-weight: bold">{{maximo}}</p>
              <b-button expanded class="is-link is-rounded" label="Minimo" type="is-light" @click="minimum" />
              <p style="margin-top: 20px; font-weight: bold">{{minimo}}</p>
              <b-button expanded class="is-warning is-rounded" label="Extraer" type="is-light" @click="extraer" />
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
              <LineChart :chartData="arrData2" :options="chartOptions" :chartColors="recoveredColors2"
                label="Humedad" />
              <b-button expanded class="is-success is-rounded" label="Promedio" type="is-light" @click="promediumHum" />
              <p style="margin-top: 20px; font-weight: bold">{{promedioHum}}</p>
              <b-button expanded class="is-info is-rounded" label="Maximo" type="is-light" @click="maximumHum" />
              <p style="margin-top: 20px; font-weight: bold">{{maximoHum}}</p>
              <b-button expanded class="is-link is-rounded" label="Minimo" type="is-light" @click="minimumHum" />
              <p style="margin-top: 20px; font-weight: bold">{{minimoHum}}</p>
              <b-button expanded class="is-warning is-rounded" label="Extraer" type="is-light" @click="extraer" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <h1>{{result}}</h1>
    <b-field label="Rounded">
      <b-input placeholder="No label" v-model="today" rounded></b-input>
    </b-field>
    <h1 v-if="resultprom !== 'NaN'">{{resultprom}}</h1>
    <h1 v-else-if="resultprom == 'NaN'">Sin resultado</h1>
    <b-button type="is-danger" label="Saka" @click="promediumtempday" />
    <h1>{{resultmax}}</h1>
    <b-button type="is-danger" label="Saka" @click="maxtempday" />
    <h1>{{resultmin}}</h1>
    <b-button type="is-danger" label="Saka" @click="mintempday" />
  </div>
</template>

<script>
  // @ is an alias to /src

  import axios from 'axios';
  import moment from "moment";
  import LineChart from "@/components/LineChart.vue";
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
      today: '',
      resultprom: 0,
      resultmax: 0,
      resultmin: 0,
      arrData: [],
      arrData2: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      },
      recoveredColors: {
        borderColor: "#032a33",
        pointBorderColor: "#032a33",
        pointBackgroundColor: "#FF0000",
        backgroundColor: "#49FF00"
      },

      recoveredColors2: {
        borderColor: "#032a33",
        pointBorderColor: "#032a33",
        pointBackgroundColor: "#49FF00",
        backgroundColor: "#FF0000"
      },
    }),
    components: {
      LineChart
    },
    methods: {
      promedium() {
        this.result.feeds.forEach(el => {
          this.promedio += parseFloat(el.field1) / parseFloat(this.result.channel.last_entry_id)
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
          this.promedioHum += parseFloat(el.field2) / parseFloat(this.result.channel.last_entry_id)
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
      promediumtempday() {
        var arrDay = 0
        var day = 0
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date == this.today) {
            arrDay += parseFloat(el.field1)
            day++
          }
        });
        this.resultprom = arrDay / day
        this.resultprom = this.resultprom.toFixed(2)
      },
      maxtempday() {
        var arrDay1 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date == this.today) {
            arrDay1.push(parseFloat(el.field1))
          }
        });
        this.resultmax = Math.max(...arrDay1)
      },
      mintempday() {
        var arrDay2 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date == this.today) {
            arrDay2.push(parseFloat(el.field1))
          }
        });
        this.resultmin = Math.min(...arrDay2)
      }
    },
    async created() {
      await axios.get("https://api.thingspeak.com/channels/1534154/feeds.json").then((result) => {
        this.result = result.data;
      })
      this.result.feeds.forEach(el => {
        const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
        //const date = el.entry_id
        this.arrData.push({
          date,
          total: parseFloat(el.field1)
        })
        this.arrData2.push({
          date,
          total: parseFloat(el.field2)
        })
      });
      console.log(this.arrData);
    },
  };
</script>