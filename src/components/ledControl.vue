<template>
    <div class="container">
        <div class="row">
             <div class="col-md-2">
               <v-led parent="led" current="red">White Led</v-led>
            </div>
            <div class="col-md-2">
               <v-led parent="led" current="blue">Blue Led</v-led>
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
          connection: '',
      }
  },
  mounted(){
      this.checkStatus();
  },
  methods: {
      checkStatus(){
          var obj = this;
          var uri = 'status';
            api.get(uri).then(function(response) {
                obj.connection = response.data['status'];
                console.log(obj.connection);
            })
      }
  }
}
</script>
<style>

</style>
