<script>

export default {
    components:{},
  data(){
    return{
      title:'',
      inputField: '',
      todoList: [],
      db:'',
      id: localStorage.getItem('id')!=null ? parseInt(localStorage.getItem('id')) : '',
    }
  },
  created(){
    let req = indexedDB.open("store6", 1);
    req.onupgradeneeded = function() {
      let db = req.result;
      if (!db.objectStoreNames.contains('notes')) {
        db.createObjectStore('notes', {keyPath: "id", autoIncrement: true});
      }
    };
    let t = this;
    req.onsuccess = function () {
      t.db = req.result;
      if(t.id != ''){
        let req = t.db.transaction(['notes'], 'readwrite').objectStore('notes').get(t.id);
        req.onsuccess = function(){
          let note = event.target.result;
          t.title = note.title;
          t.todoList = note.todoList;
        };
      }
    };
  },
  mounted(){
//    let t = this;
//    if(localStorage.getItem('id')!=null){
//      console.log(t.db);
//      let id = parseInt(localStorage.getItem('id'));
//      let notes = t.db.transaction(['notes'], 'readwrite').objectStore('notes').get(id);
//      t.title = notes.title;
//      t.todoList = notes.todoList;
//    }
  },
  methods:{
    saveNote: function () {
      let t = this;
      if(t.id==''){
        if (t.title !== '') {
          this.db.transaction(['notes'], 'readwrite').objectStore('notes').add({title:t.title,todoList:t.todoList});
  //        let openRequest = indexedDB.open("store6", 1);
  //        openRequest.onupgradeneeded = function () {
  //          console.log('создаем хранилище');
  //          let db = openRequest.result;
  //          if (!db.objectStoreNames.contains('notes')) {
  //            db.createObjectStore('notes', {keyPath: "id", autoIncrement: true});
  //          }
  //        };
  //        openRequest.onsuccess = function () {
  //          console.log('ложим в хранилище');
  //          let db = openRequest.result;
  //          let transaction = db.transaction('notes', 'readwrite');
  //          let notes = transaction.objectStore('notes');
  //          let note = {
  //            title: t.title,
  //            todoList: t.todoList
  //          };
  //          notes.add(note);
  //          let i = localStorage.getItem('i');
  //          let x = parseInt(i)+1;
  //          localStorage.setItem('i', x);
  //        };
        }
      }else {
        t.db.transaction(['notes'], 'readwrite').objectStore('notes').put({title:t.title,todoList:t.todoList,id:t.id});
      }
    },
    addTodo: function(todo) {
      todo = this.inputField;
      this.todoList.push({name: todo, complete: false});
      this.inputField = '';
   },
    deleteTodo(id, index) {
      let t = this;
      t.todoList.splice(index, 1);
      if(t.id!=''){
        t.db.transaction(['notes'], 'readwrite').objectStore('notes').put({title:t.title,todoList:t.todoList,id:t.id});
      }
    },
   toggle: function(todo) {
      todo.complete = !todo.complete;
   },
    backToList(){
      this.$emit('noteList')
    }
  },
  computed:{
  }
}
</script>

<template>
  <div>

    <div class="container">
      <div class="row">
        <div class="col-sm-6">
            <!--<form>-->
              <!--<div class="form-group">-->
                <button @click="backToList" class="btn btn-danger">Back</button>
              <!--</div>-->
            <!--</form>-->
          </div>
        <div class="col-sm-6">
            <!--<form>-->
              <!--<div class="form-group">-->
                <button @click="saveNote" class="btn btn-success">Save</button>
              <!--</div>-->
            <!--</form>-->
          </div>
      </div>
    </div>

    <div class="container">
      <form>
        <div class="form-group">
          <label>Title</label>
          <input class="form-control" v-model="title">
        </div>
        <div class="form-group">
          <label>New Todo</label>
          <input v-model="inputField" v-on:keyup.enter="addTodo" class="form-control">
        </div>
       <!--<button @click="addTodo" class="btn btn-primary">+</button>-->
      </form>

       <!--<section class="container">-->
          <!--<div class="row">-->
             <!--<div class="offset-md-3 col-md-6 mt-3">-->
                <!--<ul class="list-group justify-content-center">-->
                   <!--<li class="row list-group-item border mt-2 col-xs-1" v-for="(todo, index) in todoList">-->
                      <!--<div class="row align-items-center">-->
                        <!--<input type="checkbox" v-on:change="toggle(todo)" v-bind:checked="todo.complete" class="col-sm-1 border border-danger">-->
                        <!--<del v-if="todo.complete" class="col-sm-8">-->
                           <!--<h5>{{ todo.name }}</h5>-->
                        <!--</del>-->
                        <!--<span v-else class="col-sm-8">-->
                           <!--<h5>{{ todo.name }}</h5>-->
                        <!--</span>-->

                        <!--<span @click="deleteTodo(todo, index)" class="offset-sm-1 col-sm-2 delete text-right">X</span>-->

                      <!--</div>-->
                   <!--</li>-->
                <!--</ul>-->
             <!--</div>-->
          <!--</div>-->
       <!--</section>-->


      <div class="card">
      <ul class="list-group list-group-flush">
        <li class="list-group-item" v-for="(todo, index) in todoList">
      <!--<div class="card" >-->
        <!--<div class="card-body">-->

          <del v-if="todo.complete">
             <h6>{{ todo.name }}</h6>
          </del>
          <span v-else>
             <h6>{{ todo.name }}</h6>
          </span>
          <input type="checkbox" v-on:change="toggle(todo)" v-bind:checked="todo.complete">
          <span @click="deleteTodo(todo, index)" class="delete">X</span>

        <!--</div>-->
      <!--</div>-->
        </li>
        </ul>
        </div>


      <!--<section class="container">-->
          <!--<div class="row">-->
             <!--<div class="offset-md-3 col-md-6 mt-3">-->
                <!--<ul class="list-group justify-content-center">-->
                   <!--<li class="row list-group-item" v-for="(todo, index) in todoList">-->
                      <!--<div class="row">-->
                        <!--<input type="checkbox" v-on:change="toggle(todo)" v-bind:checked="todo.complete" class="col-sm-1 border border-danger">-->
                        <!--<del v-if="todo.complete" class="col-sm-8">-->
                           <!--<h5>{{ todo.name }}</h5>-->
                        <!--</del>-->
                        <!--<span v-else class="col-sm-8">-->
                           <!--<h5>{{ todo.name }}</h5>-->
                        <!--</span>-->

                        <!--<span @click="deleteTodo(todo, index)" class="offset-sm-1 col-sm-2 delete text-right">X</span>-->

                      <!--</div>-->
                   <!--</li>-->
                <!--</ul>-->
             <!--</div>-->
          <!--</div>-->
       <!--</section>-->

    </div>
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
    background: #4a76a8;
    cursor: hand;
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


  h1, h2 {
  font-weight: normal;
}

h5 {
   margin-bottom: 0px;
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

.delete {
   color: pink;
   cursor: pointer;
}

.delete:hover {
   color: red;
}
</style>
