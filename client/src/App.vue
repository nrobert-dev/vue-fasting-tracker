<template>
  <div v-if="loading">
    Submitting saved fast to the server..
  </div>
  <div id="pageWrapper">
    <WelcomeMessage user="Robert"/>
    <ProgressCircle
        v-bind:fastStarted="this.fastingStarted"
        v-bind:fastDuration="this.fastingDuration"
    />
    <FastingTracker
        v-on:changeFastDuration="this.setFastDuration($event)"
        v-on:changeFastingStatus="this.setFastingStatus($event)"
        v-bind:fastStarted="this.fastingStarted"
        v-bind:fastingDuration="this.fastingDuration"/>
  </div>
  <CompletedFasts v-bind:fastStarted="this.fastingStarted"/>
</template>

<script>

import FastingTracker from "./components/FastingTracker.vue";
import WelcomeMessage from "./components/WelcomeMessage.vue";
import ProgressCircle from "@/components/ProgressCircle";
import CompletedFasts from "@/components/CompletedFasts";
export default {
  name: 'App',
  data(){
    return({
      fastingStarted : false,
      fastingDuration : 16,
      loading : false
    })
  },
  components: {
    WelcomeMessage,
    FastingTracker,
    ProgressCircle,
    CompletedFasts
  },
  methods : {
    setFastingStatus(status){
      this.fastingStarted = status;

      if(status === false){
        this.submitFast(this.fastingDuration, new Date().toDateString());
      }
    },
    setFastDuration(duration){
      this.fastingDuration = duration;
    },
    submitFast(duration, date){
      fetch('http://localhost:3000/submitFast', {
        method: 'POST', // or 'PUT'
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            duration,
            date
        }),
      }).then(response => response.json()).then(() => {
        this.loading = false;
      });
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body{
  background : #100e32;
}

#pageWrapper{
  display : flex;
  background: #ee0979;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to bottom, #ff6a00, #ee0979);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to bottom, #ff6a00, #ee0979); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */



  flex-direction: column;
  box-sizing: border-box;
  padding : 30px;
  align-items: center;
  width : 480px;
  margin : 0 auto;
  border-radius : 10px;

}
</style>
