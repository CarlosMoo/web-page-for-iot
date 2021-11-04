<template>
  <div class="container">
    <div class="column is-4-desktop">
          <div class="card">
  <div class="card-image">
    <figure class="image is-4by3">
      <img src="https://bulma.io/images/placeholders/1280x960.png" alt="Placeholder image">
    </figure>
  </div>
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
    </div>
  </div>
</div>
    </div>
    <h1>{{result.channel.name}}</h1>
    <h1>{{result.feeds[0].field1}}</h1>
    <h1>{{promedio}}</h1>
    </div>
</template>

<script>
// @ is an alias to /src

import axios from 'axios';

export default {
  name: 'Home',
  data: () => ({
    result: [],
    promedio: 0,
    maximo: 0,
  }),
  components: {
  },
  methods:{
  },
  mounted() {
    axios.get("https://api.thingspeak.com/channels/1534154/feeds.json").then((result) => {
      this.result = result.data;

    for (var i = 0; i <= this.result.feeds.length; i++) {
        this.promedio += parseInt(this.result.feeds[i].field1)
    }
    this.promedio = this.promedio/this.result.feeds.length

    this.maximo = Math.max(this.result.feeds.field1)
  })
 }
};

</script>
