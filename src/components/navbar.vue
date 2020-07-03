<script>
export default {
    components:{},
  data(){
    return{
      showModal:false,
    }
  },
  mounted(){
  },
  methods:{



  },
  computed: {

  }
}
</script>

<template>
  <div>
    <!--<nav class="navbar navbar-light " role="navigation">-->
      <!--<div class="container">-->
           <!--<div class="navbar-header navbar-right">-->
           <!--</div>-->
      <!--</div>-->
    <!--</nav>-->

    <ul class="nav justify-content-center">
  <li class="nav-item">
    <button @click="createNote" class="btn btn-outline-primary btn-lg mybtn">Add Todo</button>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">Link</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
  </li>
</ul>
  </div>
</template>

<style scoped>
  .a{
    margin-left: -25px;
  }
  @media (max-width: 768px) {
    .navbar-brand {
      display: none;
    }
  }
  .navbar {
    border: none;
    background: #2C608C;
    margin-bottom: 0;
  }

  .navbar-brand {
    padding: 0px;
  }

  .general {
    margin: auto;
  }

  .navlinks {
    color: white;
  }

  .navlinks:hover {
    /*background: #4a76a8;*/
    /*cursor: hand;*/
  }

  .select:hover {
    cursor: hand;
  }

  .icon-bar {
    background: white;
  }
  a{
    padding: 22px;
  }
  .navbar-header{
    margin-top: 10px;
    margin-bottom: -10px;
  }
</style>
