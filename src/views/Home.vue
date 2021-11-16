<template>
  <div class="container">
    <section class="hero mb-4" style="background-color: #233E8B">
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
                label="Temperatura" v-if="arrData.length"/>
              <b-button expanded class="is-success is-rounded" label="Promedio" type="is-light" @click="promedium" />
              <p style="margin-top: 20px; font-weight: bold">{{promedio}}</p>
              <b-button expanded class="is-info is-rounded" label="Maximo" type="is-light" @click="maximum" />
              <p style="margin-top: 20px; font-weight: bold">{{maximo}}</p>
              <b-button expanded class="is-link is-rounded" label="Minimo" type="is-light" @click="minimum" />
              <p style="margin-top: 20px; font-weight: bold">{{minimo}}</p>
              <b-button expanded class="is-warning is-rounded is-hidden" label="Extraer" type="is-light" @click="extraer"/>
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
                label="Humedad" v-if="arrData.length"/>
              <b-button expanded class="is-success is-rounded" label="Promedio" type="is-light" @click="promediumHum" />
              <p style="margin-top: 20px; font-weight: bold">{{promedioHum}}</p>
              <b-button expanded class="is-info is-rounded" label="Maximo" type="is-light" @click="maximumHum" />
              <p style="margin-top: 20px; font-weight: bold">{{maximoHum}}</p>
              <b-button expanded class="is-link is-rounded" label="Minimo" type="is-light" @click="minimumHum" />
              <p style="margin-top: 20px; font-weight: bold">{{minimoHum}}</p>
              <b-button expanded class="is-warning is-rounded is-hidden" label="Extraer" type="is-light" @click="extraer" />
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="columns">
        <div class="column is-6-desktop">
          <div class="card">
            <div class="card-content">
              <div class="media">
                <div class="media-content">
                  <p class="title is-4">Temperatura</p>
                  <p class="subtitle is-6">Consulte los datos de un día en específico</p>
                </div>
              </div>
            </div>
            <div style="padding: 20px" class="content">
              <b-field label="Digite la fecha que quiera consultar">
                <b-input placeholder="Ej. 03/11/2021" v-model="today" rounded></b-input>
              </b-field>
              <b-button expanded class= "is-success is-rounded" type="is-light" label="Promedio" @click="promediumtempday" />
              <p style="margin-top: 20px; font-weight: bold" v-if="resultprom !== 'NaN'">{{resultprom}}</p>
              <p style="margin-top: 20px; font-weight: bold" v-else-if="resultprom == 'NaN'">Sin resultado</p>
              <b-button expanded class="is-info is-rounded" type="is-light" label="Máximo" @click="maxtempday" />
              <p style="margin-top: 20px; font-weight: bold">{{resultmax}}</p>
              <b-button expanded class="is-link is-rounded" type="is-light" label="Mínimo" @click="mintempday" />
              <p style="margin-top: 20px; font-weight: bold">{{resultmin}}</p>
            </div>
          </div>
        </div>

        <div class="column is-6-desktop">
          <div class="card">
            <div class="card-content">
              <div class="media">
                <div class="media-content">
                  <p class="title is-4">Humedad</p>
                  <p class="subtitle is-6">Consulte los datos de un día en específico</p>
                </div>
              </div>
            </div>
          <div style="padding: 20px" class="content">
            <b-field label="Digite la fecha que quiera consultar">
              <b-input placeholder="Ej. 03/11/2021" v-model="todayhum" rounded></b-input>
            </b-field>
            <b-button expanded class="is-success is-rounded" type="is-light" label="Promedio" @click="promediumhumday" />
            <p style="margin-top: 20px; font-weight: bold" v-if="resultpromhum !== 'NaN'">{{resultpromhum}}</p>
            <p style="margin-top: 20px; font-weight: bold" v-else-if="resultpromhum == 'NaN'">Sin resultado</p>
            <b-button expanded class="is-info is-rounded" type="is-light" label="Máximo" @click="maxhumday" />
            <p style="margin-top: 20px; font-weight: bold">{{resultmaxhum}}</p>
            <b-button expanded class="is-link is-rounded" type="is-light" label="Mínimo" @click="minhumday" />
            <p style="margin-top: 20px; font-weight: bold">{{resultminhum}}</p>
          </div>
        </div>
      </div>
    </div>

    <div class="columns">
      <div class="column is-6-desktop">
        <div class="card">
          <div class="card-content">
            <div class="media">
                <div class="media-content">
                  <p class="title is-4">Temperatura</p>
                  <p class="subtitle is-6">Consulte los datos de un rango de fechas en específico</p>
                </div>
            </div>
          </div>
          <div style="padding: 20px" class="content">
            <b-field label="Digite el rango de fechas que quiera consultar">
              <b-input placeholder="Fecha inicial, Ej. 03/11/2021" rounded v-model="weekstart"></b-input>
              <p class="m-2">-</p>
              <b-input placeholder="Fecha final, Ej. 10/11/2021" rounded v-model="weekfinish"></b-input>
            </b-field>
            <b-button style="margin-top: 20px;" expanded class="is-success is-rounded" type="is-light" label="Promedio" @click="weekpromediumtemp"></b-button>
            <p style="margin-top: 20px; font-weight: bold" v-if="weekresultpromtemp !== 'NaN'">{{weekresultpromtemp}}</p>
            <p style="margin-top: 20px; font-weight: bold" v-else-if="weekresultpromtemp == 'NaN'">Sin resultado</p>
            <b-button expanded class="is-info is-rounded" type="is-light" label="Maximo" @click="weekmaxtemp"></b-button>
            <p style="margin-top: 20px; font-weight: bold">{{weekresultmaxtemp}}</p>
            <b-button expanded class="is-link is-rounded" type="is-light" label="Minimo" @click="weekmintemp"></b-button>
            <p style="margin-top: 20px; font-weight: bold">{{weekresultmintemp}}</p>
          </div>
        </div>
      </div>

      <div class="column is-6-desktop">
        <div class="card">
          <div class="card-content">
            <div class="media">
                <div class="media-content">
                  <p class="title is-4">Humedad</p>
                  <p class="subtitle is-6">Consulte los datos de un rango de fechas en específico</p>
                </div>
            </div>
          </div>
          <div style="padding: 20px" class="content">
            <b-field label="Digite el rango de fechas que quiera consultar">
              <b-input placeholder="Fecha inicial, Ej. 03/11/2021" rounded v-model="weekstarthum"></b-input>
              <p class="m-2">-</p>
              <b-input placeholder="Fecha final, Ej. 10/11/2021" rounded v-model="weekfinishhum"></b-input>
            </b-field> 
            <b-button style="margin-top: 20px;" expanded class="is-success is-rounded" type="is-light" label="Promedio" @click="weekpromediumhum"></b-button>
            <p style="margin-top: 20px; font-weight: bold" v-if="weekresultpromhum !== 'NaN'">{{weekresultpromhum}}</p>
            <p style="margin-top: 20px; font-weight: bold" v-else-if="weekresultpromhum == 'NaN'">Sin resultado</p>
            <b-button expanded class="is-info is-rounded" type="is-light" label="Maximo" @click="weekmaxhum"></b-button>
            <p style="margin-top: 20px; font-weight: bold">{{weekresultmaxhum}}</p>
            <b-button expanded class="is-link is-rounded" type="is-light" label="Minimo" @click="weekminhum"></b-button>
            <p style="margin-top: 20px; font-weight: bold">{{weekresultminhum}}</p>
          </div>
        </div>
      </div>
    </div>

    <div class="columns">
      <div class="column is-12-desktop">
        <div class="card mb-3">
          <div class="card-content">
              <div class="media">
                <div class="media-content">
                  <p class="title is-4">Tabla de Mediciones</p>
                  <p class="subtitle is-6">Consulte y ordene las mediciones realizadas</p>
                </div>
              </div>
            <b-table :data="result.feeds"
              bordered
              striped
              narrowed
              hoverable
              mobile-cards
            >
              <b-table-column field="entry_id" label="ID" sortable v-slot="props" centered>
                {{ props.row.entry_id }}
              </b-table-column>
              <b-table-column field="created_at" label="Fecha" sortable v-slot="props" centered>
                {{ props.row.created_at.substring(0, 10) }}
              </b-table-column>
              <b-table-column field="field1" label="Temperatura" sortable v-slot="props" centered>
                {{ props.row.field1 }}
              </b-table-column>
              <b-table-column field="field2" label="Humedad" sortable v-slot="props" centered>
                {{ props.row.field2 }}%
              </b-table-column>
            </b-table>
          </div>
        </div>
      </div>
    </div>

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
      todayhum: '',
      resultprom: 0,
      resultmax: 0,
      resultmin: 0,
      resultpromhum:0,
      resultmaxhum: 0,
      resultminhum: 0,
      weekstart: '',
      weekfinish: '',
      weekstarthum: '',
      weekfinishhum: '',
      weekresultpromtemp: 0,
      weekresultmaxtemp: 0,
      weekresultmintemp: 0,
      weekresultpromhum: 0,
      weekresultmaxhum: 0,
      weekresultminhum: 0,
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
      },
      promediumhumday() {
        var arrDay = 0
        var day = 0
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date == this.todayhum) {
            arrDay += parseFloat(el.field2)
            day++
          }
        });
        this.resultpromhum = arrDay / day
        this.resultpromhum = this.resultpromhum.toFixed(2)
      },
      maxhumday() {
        var arrDay1 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date == this.todayhum) {
            arrDay1.push(parseFloat(el.field2))
          }
        });
        this.resultmaxhum = Math.max(...arrDay1)
      },
      minhumday() {
        var arrDay2 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date == this.todayhum) {
            arrDay2.push(parseFloat(el.field2))
          }
        });
        this.resultminhum = Math.min(...arrDay2)
      },
      weekpromediumtemp() {
        var arrWeek = 0
        var week = 0
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date >= this.weekstart && date <= this.weekfinish) {
            arrWeek += parseFloat(el.field1)
            week++
          }
        });
        this.weekresultpromtemp = arrWeek / week
        this.weekresultpromtemp = this.weekresultpromtemp.toFixed(2)
      },
      weekmaxtemp() {
        var arrWeek1 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date >= this.weekstart && date <= this.weekfinish) {
            arrWeek1.push(parseFloat(el.field1))
          }
        });
        this.weekresultmaxtemp = Math.max(...arrWeek1)
      },
      weekmintemp() {
        var arrWeek2 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date >= this.weekstart && date <= this.weekfinish) {
            arrWeek2.push(parseFloat(el.field1))
          }
        });
        this.weekresultmintemp = Math.min(...arrWeek2)
      },
      weekpromediumhum() {
        var arrWeek = 0
        var week = 0
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date >= this.weekstarthum && date <= this.weekfinishhum) {
            arrWeek += parseFloat(el.field2)
            week++
          }
        });
        this.weekresultpromhum = arrWeek / week
        this.weekresultpromhum = this.weekresultpromhum.toFixed(2)
      },
      weekmaxhum() {
        var arrWeek1 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date >= this.weekstarthum && date <= this.weekfinishhum) {
            arrWeek1.push(parseFloat(el.field2))
          }
        });
        this.weekresultmaxhum = Math.max(...arrWeek1)
      },
      weekminhum() {
        var arrWeek2 = []
        this.result.feeds.forEach(el => {
          const date = moment(el.created_at.substring(0, 10), "YYYY-MM-DD").format("DD/MM/YYYY")
          if (date >= this.weekstarthum && date <= this.weekfinishhum) {
            arrWeek2.push(parseFloat(el.field2))
          }
        });
        this.weekresultminhum = Math.min(...arrWeek2)
      },
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