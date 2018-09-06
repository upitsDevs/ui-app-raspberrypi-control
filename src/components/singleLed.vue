<template>
    <button class='btn btn-lg btn-block' v-bind:class="[{ 'btn-active': stat , 'btn-inactive': !stat }]" v-on:click='toggleLed'>
        <i class="fa fa-power-off fa-4x"></i><h3><slot>Placeholder</slot></h3>
    </button>
</template>
<script>
const axios = require('axios');
const api = axios.create({
  baseURL: 'http://192.168.2.107:5000/',
  timeout: 1000,
  headers: {'Content-Type': 'application/json'}
});
export default {
    data() {
        return{
            stat: 0,
        }
    },
    mounted() {
        this.getLed()
    },
    methods: {
        getLed(){
            var obj = this;
            var uri = this.parent + '/' + this.current + '/';
            api.get(uri).then(function(response) {
                obj.stat = response.data[obj.current];
            })
        },
        toggleLed(color) {
            var self = this;
            var uri = this.parent + '/' + this.current + '/';
            if(self.stat){
                var stateToChange = '0';
            }else{
                var stateToChange = '1';
            }
                api.post(uri , {
                    'state' : stateToChange ,
                })
                    .then(function (response) {
                    self.getLed();
                })
            
        },
    },
    props: {
        parent: {
            type: String,
        },
        current: {
            type: String,
        }
    }
  }
</script>
