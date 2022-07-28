<template>
  <div class="todo">
    <h1  class="text-center fw-bolder my-5 text-warning">
        <i class="material-icons"> list </i> 
           Liste des taches 
     </h1>
    <form  class="form shadow d-flex m-5 p-5 rounded"  @submit.prevent="Onsubmit">
      <div class="container">
        <input
          type="text"
          v-model="info"
          class="form-control mb-3 p-4"
          placeholder="Enter name Your Tasks"
        />
        <input
          type="text"
          v-model="description"
          class="form-control mb-3 p-4"
          placeholder="Enter Your description"
        />
        <button v-if="!edit" class="btn btn-light p-3 btn-outline-warning">
          Add
        </button>
        <button v-else class="btn btn-light p-3 btn-outline-success">
          Update
        </button>
      </div>
    </form>

    <div v-if="trouve==0" class="container">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Name</th>
            <th scope="col">description</th>
            <th scope="col">Etat</th>
            <th>Operation</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(todo, index) in todos" :key="todo.id">
            <th scope="row">{{ index + 1 }}</th>
            <td :class="todo.etat ? 'decoration' : 'fw-bolder'">
              {{ todo.name }}
            </td>
            <td :class="todo.etat ?'decoration' :'fw-bolder'" >{{todo.description}}</td>
            <td
              v-if="todo.etat"
              class="text-danger"
              @click="changer_etat(index)"
              style="cursor: pointer"
            >
              {{ todo.etat ? "complete" : "incolmplet" }}
            </td>
            <td
              v-else
              class="text-succes"
              @click="changer_etat(index)"
              style="cursor: pointer"
            >
              {{ todo.etat ? "complete" : "incolmplet" }}
            </td>
            <td>
              <button @click="editTodo(index)" class="btn btn-warning m-1">
                Edit
              </button>
              <button @click="remove(index)" class="btn btn-danger m-1">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
   
    </div>
     <div v-else class="container">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Name</th>
            <th scope="col">description</th>
            <th scope="col">Etat</th>
            <th>Operation</th>
          </tr>
        </thead>
        <tbody>
          <tr >
            <th scope="row">{{ this.id+1}}</th>
            <td :class="this.todos[this.id].etat ? 'decoration' : 'fw-bolder'">
              {{ this.todos[this.id].name }}
            </td>
            <td :class="this.todos[this.id].etat ?'decoration' :'fw-bolder'" >{{this.todos[this.id].description}}</td>
            <td
             :class="this.todos[this.id].etat ? 'text-danger' :'text-success'"
              @click="changer_etat(this.id+1)"
              style="cursor: pointer"
            >
              {{ this.todos[this.id].etat ? "complete" : "incolmplet" }}
            </td>
            <td>
              <button @click="editTodo(this.id)" class="btn btn-warning m-1">
                Edit
              </button>
              <button @click="remove(this.id)" class="btn btn-danger m-1">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
       <input v-model="search"  type="text" id="search" placeholder="Search Here With name or description ">
       <button class="btn btn-info p-2 m-2" @click="recherche">search</button>
       <br>
       <small style="font-size:20px;margin-left:-18px" :class="this.trouve ? 'text-success' : 'text-danger'" >{{this.erreur}}</small>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    todos: Array,
  },
  data() {
    return {
      info: "",
      filterage: "",
      select: "",
      edit: 0,
      description: "",
      search:"",
      id:-1,
      trouve:0,
      erreur:''
    };
  },
  methods: {
    remove(index) {
      this.$emit("delete_todo", index);
    },
    Onsubmit() {
      var a = 0;
      if (this.info === "") {
        alert("invalid");
        return;
      }
      this.todos.forEach((v) => {
        if (v.name.toUpperCase() === this.info.toUpperCase()) {
           
          if(v.etat===1){
              alert("task compelet");
          }else{
          alert("deja existe");
          }
          a = 1;
          return;
        }
      });
      if (a == 0) {
        if (this.edit == 0) {
          let todo = {
            id: this.todos.length + 1,
            name: this.info,
            description:this.description,
            etat: 0,
          };
          this.$emit("add_todo", todo);
          this.info = "";
          this.description="";
        } else {
          let todo = {
            id: this.select,
            name: this.info,
            description:this.description,
            etat: this.todos[this.select].etat,
          };
          this.$emit("edit_todo", todo, this.select);
          this.info = "";
          this.description="";
          this.edit = 0;
        }
      }
    },
    editTodo(index) {
      this.info = this.todos[index].name;
      this.description=this.todos[index].description;
      this.edit = 1;
      this.select = index;
    },
    changer_etat(index) {
      this.$emit("onchange", index, this.info);
    },
    recherche(){
      this.todos.forEach(v=>{
        if(v.name.toUpperCase()===this.search.toUpperCase() || v.description.toUpperCase()===this.search.toUpperCase()){
          this.trouve=1;
          this.id=v.id-1;
         }else{
           if(this.search===''){
            this.erreur="champ empty!";
           }
           else if(this.trouve===0) {
             this.erreur="doesn't exist";
           }else {
              this.erreur=" exist "+this.search;
           }
         }
      })  
   },
    //  edit_todo(todo) {
    // 	this.edit_id = todo.id;
    // 	this.info = todo.name;
    // 	this.edit = true;
    // },
  },
  components: {

},
};
</script>
<style>
* {
  background-color: whitesmoke;
}
.decoration {
  text-decoration: line-through;
}
#search{
  height:40px;
  text-align: center;
  width: 350px;
  padding-left:10px;
  border: none;
  border-bottom:2px solid green;
  outline: none;
  font-size: 18px;
}
</style>