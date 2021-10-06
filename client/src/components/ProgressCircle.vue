<template>
  <radial-progress-bar diameter="250"
                       strokeWidth="20"
                       startColor="#9a20e6"
                       stopColor="#ee0979"
                       :completed-steps="completedSteps"
                       :total-steps="totalSteps">
    <p>{{ countdownMessage && countdownMessage }}</p>
  </radial-progress-bar>
</template>

<script>
import RadialProgressBar from 'vue-radial-progress';




export default {
  name : "ProgressCircle",
  data () {
    return {
      completedSteps: 0,
      totalSteps: null,
      strokeWidth : 20,
      interval : null,
      timeLeft : null,
      fastEndDate : null,
      countdownMessage : "No Active Fasts"
    }
  },
  props : {
    fastStarted : Boolean,
    fastDuration : Number,
  },
  watch : {
    fastStarted : function(){
      if(this.fastStarted){
        let date = new Date();
        date.setHours(date.getHours() + this.fastDuration);
        this.fastEndDate = date;
        this.countdownMessage = `${this.fastDuration} hours, 0 minutes`;
        this.totalSteps = this.fastDuration * 60 * 60;

        this.interval = setInterval(() => {
          let diffInMilliSeconds = Math.abs(this.fastEndDate - new Date()) / 1000;

          // calculate hours
          const hours = Math.floor(diffInMilliSeconds / 3600) % 24;
          diffInMilliSeconds -= hours * 3600;

          // calculate minutes
          const minutes = Math.floor(diffInMilliSeconds / 60) % 60;
          diffInMilliSeconds -= minutes * 60;

          // calculate minutes
          const seconds = Math.floor(diffInMilliSeconds % 60);
          diffInMilliSeconds -= seconds * 60;

          let difference = '';
          difference += (hours === 0 || hours === 1) ? `${hours} h, ` : `${hours} h, `;
          difference += (minutes === 0 || hours === 1) ? `${minutes} m` : `${minutes} m`;
          difference += (seconds === 0 || minutes === 1) ? `${seconds} s` : `${seconds} s`;

          this.countdownMessage = difference;
          this.completedSteps+=2;
        }, 2000);
      }
      else{
        this.fastEndDate = null;
        this.countdownMessage = 'No Active Fasts';
        clearInterval(this.interval);
      }

    }
  },
  components: {
    RadialProgressBar
  }
}
</script>

<style scoped>
  p{
    color : white;
    font-size : 18px;
    font-weight : 800;
  }

</style>