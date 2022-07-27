<template>
  <div class="todo">
      <h1 class="text-center fw-bolder my-5 text-warning">Liste des taches</h1>
       <form class="form shadow d-flex m-5 p-5 rounded"  @submit.prevent="Onsubmit">
         <div class="container">
         <input type="text" v-model="info" class="form-control mb-3 p-4" placeholder="Enter Your Tasks">
         <button class="btn btn-light  p-3 btn-outline-warning ">Add</button>
         </div>
       </form>

     <div class="container">  
          <table class="table">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">Etat</th>
                <th>Operation</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="todo in todos" :key="todo.id">
                <th scope="row">{{todo.id}}</th>
                
                <td v-if="todo.etat" class="text-decoration-line-through">{{todo.name}}</td>
                <td v-else class="fw-bold">{{todo.name}}</td>

                <td  v-if="todo.etat" class="text-danger" @click="changer_etat(todo.id)"  style="cursor:pointer">{{todo.etat ? 'complete':'incolmplet'}}</td>
                <td v-else class="text-succes" @click="changer_etat(todo.id)"  style="cursor:pointer">{{todo.etat ? 'complete':'incolmplet'}}</td>
                
                <td>
                  <button @click="edit(todo.id)" class="btn btn-warning">Edit</button>
                  <button @click="remove(todo.id)" class="btn btn-danger">Delete</button>
                </td>
              </tr>
          </tbody>
        </table>
    </div>
  </div>
</template>

<script>
  export default {
    name:'HelloWorld',
    props:{
    todos:Array
     },
  data(){
   return{
      info:'',
      filterage:''
   }
  },
  methods: {
    remove(index){
      this.$emit('delete_todo',index);
    },
    Onsubmit(){
       if(this.info===''){
         alert('invalid');
         return;
       }
        let todo={
           id:this.todos.length+1,
           name:this.info,
           etat:0
        }
         this.$emit('add_todo',todo);
         this.info='';
    },
    edit(){
      // 
    },
    changer_etat(index){
      this.$emit('onchange',index);
    },
    
  },
  computed:{
   
  }

  }
  

</script>

<style>
 *{
  background-color: whitesmoke;
 }
</style>