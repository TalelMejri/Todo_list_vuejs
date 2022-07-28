<template>
  <div id="app">
     <HelloWorld :todos="todos" @delete_todo="deletee" @add_todo="add" @modifier="edit" @onchange="changer" ></HelloWorld>
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
      todos:[{id:1,name:"HTML",etat:1},
              {id:2,name:"VueJs",etat:0},
              {id:3,name:"CSS",etat:1},
              {id:4,name:"Bootstrap",etat:0}
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
    },
    changer(index){
      this.todos[index].etat = this.todos[index].etat ? 0 : 1 ;
    }
  },
  components:{
    HelloWorld,footer_end
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
