<template>
  <div class="wrapper">
    <h2>Completed Fasts</h2>

    <div v-if="loading" class="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <ul v-if="fasts" id="example-1">
      <li v-for="fast in fasts" :key="fast.duration">
        {{ fast.duration }} Hours - {{ fast.date}}
      </li>
    </ul>
  </div>
</template>

<script>
export default{
  name : "Completed Fasts",
  data(){
    return({
      loading : false,
      fasts : null,
      error : null
    })
  },
  created() {
    this.fetchData();
  },
  props: {
    fastStarted : Boolean
  },
  watch: {
    fastStarted: function() {
        this.fetchData();
    }
  },
  methods : {
    fetchData(){
      this.error = this.post = null;
      this.loading = true;

      fetch('http://localhost:3000/fasts')
          .then(response => response.json())
          .then(json =>{
            this.loading = false;
            this.fasts = json.slice(0,5);
          });
    }
  }
}
</script>

<style scoped>
.wrapper{
  width : 70%;
  background : white;
  border-radius : 8px;
  margin : 0 auto;
}
</style>

