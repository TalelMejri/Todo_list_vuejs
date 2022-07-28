<template>
  <div class="todo">
      <h1 class="text-center fw-bolder my-5 text-warning">Liste des taches</h1>
       <form class="form shadow d-flex m-5 p-5 rounded"  @submit.prevent="Onsubmit">
         <div class="container">
         <input type="text" v-model="info" class="form-control mb-3 p-4" placeholder="Enter Your Tasks">
         <button v-if="verifier" class="btn btn-light  p-3 btn-outline-warning ">Add</button>
         <button @click="update" v-else class="btn btn-light  p-3 btn-outline-success ">Update</button>
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
              <tr v-for="(todo,index) in todos" :key="todo.id">
                <th scope="row">{{index+1}}</th>
                
                <td v-if="todo.etat" class="text-decoration-line-through">{{todo.name}}</td>
                <td v-else class="fw-bold">{{todo.name}}</td>

                <td  v-if="todo.etat" class="text-danger" @click="changer_etat(index)"  style="cursor:pointer">{{todo.etat ? 'complete':'incolmplet'}}</td>
                <td v-else class="text-succes" @click="changer_etat(index)"  style="cursor:pointer">{{todo.etat ? 'complete':'incolmplet'}}</td>
                
                <td>
                  <button @click="edit(index)" class="btn btn-warning">Edit</button>
                  <button @click="remove(index)" class="btn btn-danger">Delete</button>
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
      filterage:'',
      select:'',
      verifier:1
   }
  },
  methods: {
    remove(index){
      this.$emit('delete_todo',index);
    },
    Onsubmit(){
      var a=0;
       if(this.info===''){
         alert('invalid');
         return;
       }
       this.todos.forEach(v=>{
        if(v.name.toUpperCase()===this.info.toUpperCase()){
          alert("deja existe");
          a=1;
          return;
        }
       })
        if(a==0){
           if(this.verifier==0){
        let todo={
           id:this.todos.length+1,
           name:this.info,
           etat:0
        }
         this.$emit('add_todo',todo);
         this.info='';
           }else{

          let todo={
           id:this.select,
           name:this.info,
           etat:this.todo[this.select].etat
        }
         this.$emit('onchange',todo,this.select);
         this.info='';
         this.verifier=1;
        }
        }
    },
    edit(index){
      this.info=this.todos[index].name;
      this.verifier=0;
      this.select=index;
    },
    changer_etat(index){
      this.$emit('onchange',index,this.info);
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