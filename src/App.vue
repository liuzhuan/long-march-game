<template>
  <index v-if='state == 0' transition="fade"></index>  
  
  <!-- rememeber to alter it back to v-show -->
  <quiz v-show='state == 1' v-if='showQuiz' transition="fade"></quiz>

  <game v-if='state == 2' transition="fade"></game>
  
  <!-- transition is ignored on component <hero> because the component is a fragment instance -->
  <hero v-if='state == 3' transition='fade'></hero>
  
  <board v-if='state == 4' transition='fade'></board>

</template>

<script>
import Index from './components/Index'
import Quiz from './components/Quiz'
import Game from './components/Game'
import Hero from './components/Hero'
import Board from './components/Board'

export default {
  data(){
    return {
      state: 0,
      showQuiz: false
    }
  },

  components: {
    Index,
    Quiz,
    Game,
    Hero,
    Board
  },

  created(){
    // console.log("App.vue created!");
    this.$on('showQuiz', function(){
      console.log('App.vue show Quiz Handler')
      this.state = 1
      this.showQuiz = true
    })

    this.$on('showGame', function(){
      console.log('[App.vue] showGame')
      this.state = 2
    })

    this.$on('nextQuiz', ()=>{
      console.log('[App.vue] showNextQuiz')
      
      this.$broadcast('nextQuiz')
      this.state = 1
    })

    this.$on('showHero', ()=>{
      this.state = 3
      this.showQuiz = false
    })

    this.$on('showBoard', ()=>{
      this.state = 4
    })
  }
}
</script>

<style src="./css/style.css">
</style>
