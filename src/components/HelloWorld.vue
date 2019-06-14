<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>{{ percent }}</h2>
    <progress-bar :style="progressStyle" leftBgColor="#666" barBgColor="#cccccc" ballBgColor="#3d9b98" percent="15%" @bar-drag="drag"></progress-bar>
  </div>
</template>

<script>
/* eslint-disable */
import ProgressBar from './ProgressBar'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      progressStyle:{
        width: '80%',
        margin: '0 auto'
      },
      percent:''
    }
  },
  components:{
    'progress-bar':ProgressBar
  },
  methods:{
    drag(percent){
      this.percent = percent;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
