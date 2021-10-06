<template>
  <h2>VUE FASTING</h2>
  <h4>Welcome to the app, {{ user }}!</h4>
</template>

<script>
  export default{
    name : "Welcome Message",
    props : {
      user : String
    }
  }
</script>

<style scoped>
  h2{
    color : #fffcfc;
    font-size : 25px;
    margin : 0;
  }
  H4{
    color : #fafafa;
    margin : 10px 0px 20px 0px;

  }
</style>

