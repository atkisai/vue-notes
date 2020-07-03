<template>
  <div id="app">
    <!--<navBar></navBar>-->
    <createNote v-if="state=='createNote'" @noteList="noteList"></createNote>
    <noteList v-if="state=='noteList'" @createNote="createNote"></noteList>
  </div>
</template>

<script>
import navBar from './components/navbar.vue'
import createNote from './components/createNote.vue'
import noteList from './components/noteList.vue'

export default {
  name: 'App',
  components: {createNote, noteList, navBar},
  created(){
    if(localStorage.getItem('i')==null){localStorage.setItem('i','0')}
  },
  mounted(){
  },
  data(){
    return{
      state:'noteList',
    }
  },
  methods:{
    createNote(){
      this.state='createNote'
    },
    noteList(){
      this.state='noteList'
    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
}
</style>
