<template>
  <div id="app">
     <HelloWorld :todos="todos" @delete_todo="deletee" @add_todo="add" @edit_todo="edit" @onchange="changer" ></HelloWorld>
      <footer_end></footer_end>
  </div>
</template>
<script>
 import HelloWorld from "./components/HelloWorld.vue";
 import footer_end from "./components/footer_end.vue";

export default {
  name:'app',
  data(){
    return{
      todos:[{id:1,name:"HTML",description:'video Youtube',etat:1},
              {id:2,name:"VueJs",description:'projet',etat:0},
              {id:3,name:"CSS",description:'video Youtube',etat:1},
              {id:4,name:"Bootstrap",description:'code projet',etat:0}
            ]
    }
  },
  methods:{
    deletee(index){
      this.todos.splice(index,1);
    },
    add(todo){
      this.todos.push(todo);
    },
    edit(todo,index){
      this.todos[index]=todo;
    //   let index = this.todos.findIndex(t => t.id === todo.id);
		//   this.todos[index].name = todo.name;
    },
    changer(index){
      this.todos[index].etat = this.todos[index].etat ? 0 : 1 ;
    }
  },
  components:{
    HelloWorld,
    footer_end,
}
}
</script>
<style>
 @import 'https://fonts.googleapis.com/icon?family=Material+Icons';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
