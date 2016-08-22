<template>
  <div class="page index-page">
    <div class="btn btn-start" @click="showQuiz"></div>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  data () {
    return {
      // note: changing this line won't causes changes
      // with hot-reload because the reloaded component
      // preserves its current state and we are modifying
      // its initial state.
      msg: 'Index!'
    }
  }, 

  methods: {
    showQuiz(){
      this.$dispatch('showQuiz', 'hi');
    }
  },

  created(){
    // console.log("Index.vue created");
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.index-page {
  color: #42b983;
  width: 1078px;
  height: 536px;
  margin-left: -550px;
  margin-top: -268px;
  background-image: url(../assets/bg-pop-start.png);
}

.btn-start {
  position: absolute;
  left: 450px;
  top: 350px;
  width: 174px;
  height: 47px;
  background-image: url(../assets/btn-start.png);
}
</style>
