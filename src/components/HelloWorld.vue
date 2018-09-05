<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
      <b-button v-on:click="getLed()">Get Status</b-button>
      <b-button v-on:click="switchOn()">Switch On</b-button>
      <b-button v-on:click="switchOff()">Switch Off</b-button>

      <b-button v-on:click="toggleLed()">Toggle</b-button>
  </div>
</template>

<script>
import bButton from 'bootstrap-vue/es/components/button/button';
const axios = require('axios');
const api = axios.create({
  baseURL: 'http://192.168.2.107:5000/',
  timeout: 1000,
  headers: {'Content-Type': 'application/json'}
});
export default {
  name: 'HelloWorld',
  components: {
    'b-button': bButton,
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      state: 0,
      flag: 0,
    }
  },
  mounted() {
  },
  methods: {
    getLed(){
      axios.get('http://192.168.2.107:5000/led/blue/')
        .then(function (response) {
          console.log(response.data);
        })
    },
    switchOn(){
      axios.post('http://192.168.2.107:5000/led/blue/', {
        'state' : 1 ,
      })
        .then(function (response) {
          console.log(response);
          this.getLed();
        })
    },
    switchOff(){
      axios.post('http://192.168.2.107:5000/led/blue/', {
        'state' : 0 ,
      })
        .then(function (response) {
          console.log(response);
          this.getLed();
        })
    },
    toggleLed(ledNum){
        if(this.getLed()){
          this.flag = 0
        }else{
          this.flag = 1
        }
        axios.post('http://192.168.2.107:5000/led/' . ledNum , {
          'state': this.flag,
        }).then(function(response){
          console.log(response.data)
        })
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
