<template>
    <div class="container mainWidget">
        <div class="row">        
            <div class="col-lg-4 infoWidget">            
                <img src="../../static/icons/temp.png" class='img-fluid' v-bind:class="[{ 'invert': color }]">
                <h3>Temperature : <small>{{ temp }}</small> </h3>
            </div>
            <div class="col-lg-4 infoWidget">            
                <img src="../../static/icons/humidity.png" class='img-fluid' v-bind:class="[{ 'invert': color }]">
                <h3>Humidity : <small>{{ humidity }} %</small></h3>
            </div>
            <div class="col-lg-4 infoWidget">            
                <img src="../../static/icons/wifi.png" class='img-fluid' v-bind:class="[{ 'invert': color }]">
                <h3>Connectivity : <small>{{ connection }}</small></h3>
            </div>
        </div>
        <div class="row mt-5 mt-20 pt-5">
             <div class="col-md-3 actionWidget">
               <v-led parent="led" current="red">Light</v-led>
            </div>
            <div class="col-md-3 actionWidget">
               <v-led parent="led" current="blue">Low Light</v-led>
            </div>
            <div class="col-md-3 actionWidget">
               <v-led parent="led" current="ac">A/C</v-led>
            </div>
            <div class="col-md-3 actionWidget">
               <v-led parent="led" current="door">Door</v-led>
            </div>
        </div>
        <div class="row footer mt-5 pt-5">
            <div class="col-lg-6 mt-5">
                <button class='btn btn-success btn-sm pull-left' v-on:click="checkStatus"><i class="fa fa-refresh"></i></button>
                <span class='lastUpdate pull-left'>last update: {{ lastUpdate }}</span>
            </div>
            <div class="col-lg-6 mt-5">
                <p class='copyright pull-right'>Powered By <i class="fa fa-microchip"></i> <span>Upits.IO</span> Enterprise Smart Solutions</p>
            </div>
        </div>
    </div>
</template>
<script>
const axios = require('axios');
const api = axios.create({
  baseURL: 'http://192.168.2.107:5000/',
  timeout: 1000,
  headers: {'Content-Type': 'application/json'}
});
import singleLed from '@/components/singleLed';
export default {
  name: 'ledControl',
  components: {
      'v-led':singleLed,
  },
  data(){
      return{
          connection: 'Disconnected',
          status: '',
          temp: 'N/A',
          humidity: 'N/A',
          lastUpdate:'N/A'
      }
  },
  mounted(){
      this.checkStatus();
  },
  methods: {
      checkStatus(){
          var obj = this;
          var uri = 'temp';
            api.get(uri).then(function(response) {
                obj.connection = response.data['status'];
                obj.temp = response.data['temperature'];
                obj.humidity = response.data['humidity'];
                obj.lastUpdate = response.data['last_update'];
            })
      },
      color() {
          if(this.connection == 'connected') {
              this.status = true;
          }else{
              this.status = false;
          }
      }
  }
}
</script>
<style>

</style>
