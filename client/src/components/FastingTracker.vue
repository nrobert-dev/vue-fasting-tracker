<template>
  <div class="mainWrapper">
    <p v-if="!fastStarted">Choose your fasting duration</p>

    <div v-show="!fastStarted" class="typeWrapper">
      <div v-for="type in fastingTypes" :key="type"
           class="typeCard"
           :class="{activeFast : type === fastingDuration}"
            @click="setFastDuration($event, type)">
        {{ type }} Hours
      </div>
    </div>

    <button v-if="!fastStarted" @click="setFastStatus($event, true)" :class="{ disabled : fastStarted}">Start Your Fast</button>
    <button v-else @click="setFastStatus($event, false)" :class="{ disabled : fastStarted}">End Your Fast</button>

  </div>
</template>

<script>
  export default {
    name: 'FastingTracker',
    data(){
      return{
        fastingTypes : [14,16,18,20,22,24]
      }
    },
    props: {
      fastingDuration: Number,
      fastStarted : Boolean
    },
    methods : {
      setFastStatus(_event, status){
        this.$emit('changeFastingStatus', status);
      },
      setFastDuration(_event,duration){
        this.$emit('changeFastDuration', duration);
      }
    }
  }
</script>

<style scoped>
  .mainWrapper{
    display : flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }

  .disabled {
    color : #292929;
    background : #eeeeee;
  }

  .typeWrapper{
    display:flex;
    flex-direction: row;
  }

  .typeCard{
    font-size: 12px;
    width : 50px;
    height : 50px;
    background : #fcfcff;
    border-radius : 6px;
    box-sizing: border-box;
    margin: 5px;
    font-weight : 600;
    padding : 13px 0;
    cursor : pointer;
    color : #2d2d2d;
  }

  .activeFast{
    background : #ff6325;
  }

  button {
    margin-top : 20px;
    border-radius : 30px;
    width : 160px;
    height : 35px;
    border : none;
    padding : 5px;
    font-weight : 600;
    color : white;
    background : #4d3fd0;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  }

  button:hover{
    background : #695ce5;
  }

  button:active{
    background : gray;
  }

  .disabled:hover {
    background : #eed6d6;
  }

  p{
    color : white;
    font-size : 14px;
    margin : 10px 0px 3px 0px;
  }

</style>