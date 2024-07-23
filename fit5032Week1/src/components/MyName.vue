<template>
    <div>
      <h1>{{ message }}</h1>
    </div>
  </template>
  
  <script>
  export default {
    name: 'MyName',
    data() {
      return {
        message: 'Welcome, Lachlan Williams!'
      }
    }
  }
  </script>
  
  <style scoped>
  h1 {
    color: #42b983;
  }
  </style>
  