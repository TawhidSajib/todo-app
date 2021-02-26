<template>
<v-container>
    <v-form>
    <v-row justify="center">
        <v-col
          cols="12"
          sm="6"
          md="4"
        >
          <v-text-field
            dense
            label="Todo"
            v-model="todo"
            v-on:keyup.enter="submit"
          ></v-text-field>
          
        </v-col>
       
         

    </v-row> 
    <v-row justify="center">
        <v-col md="4" >
    <v-btn style="margin-left: 2px" outlined color="success" @click="filterTodo = 'active'" >Active</v-btn>
    <v-btn outlined color="success" @click="filterTodo = 'complete'" >Complete</v-btn>
    <v-btn outlined color="success" @click="filterTodo = 'allSelect'">All Data</v-btn>
        </v-col>
    </v-row>
    <v-btn color="primary" v-if="!isEditing" @click="submit">Add</v-btn>
    <v-btn v-else @click="updateData">Update</v-btn>
    <v-btn @click="allData">All Data</v-btn>
    
    <v-btn @click="allDelete">Delete All Complete Data</v-btn>
    </v-form>
    <pre>
        {{computedFilter}}
        {{todos}}
    </pre>
   
        <ol>
            <li v-for="(todo,index) in  computedFilter" :key="index">
                <v-checkbox value :checked="todo.complete"  v-on:change="completeTask(todo)"></v-checkbox>
                <span :class="{completed: todo.complete}">{{  todo.title }}</span>
                
                <v-btn @click="edit(todo,index)">Edit</v-btn>
                <v-btn @click="remove(index)">Remove</v-btn>
            </li>
       </ol>     
</v-container>
  
</template>
<script>
export default {
  data(){
    return {
     todo: '',
     todos: [{title: 'Go Home',complete: false},{title: 'return Home',complete: false}],
     isEditing: false,
     selectIndex: '',
     filterTodo: ''
    }
  },
 computed:{
       computedFilter(){
         if(this.filterTodo === 'active'){
             return this.todos.filter(todo=> todo.complete === false)
         }else if (this.filterTodo === 'complete'){
             return this.todos.filter(todo=> todo.complete === true)
         }else if(this.filterTodo === 'allSelect'){
             return this.todos
         }
         else {
             return this.todos
         }
       }
 },
 mounted(){
     if(localStorage.todos){
    this.todos = JSON.parse(localStorage.todos)
     } 
      
 },
 watch: {
       todos: {
           handler(newTodos){
               localStorage.todos = JSON.stringify(newTodos)
           },
           deep: true
       }
 },
  methods: {
      submit(){
          if(this.todo && this.todo.trim() != ''){
              this.todos.push({
              title: this.todo,
              complete: false
          })
          this.todo = ''

          }
          
      },
      remove(id){
          this.todos.splice(id,1)
      },
      edit(data,id){
          this.isEditing = true
          this.todo = data.title
          this.selectIndex = id
      },
      updateData(){
          this.isEditing = false
          console.log(this.todo)
          console.log(this.selectIndex)
          this.todos.splice(this.selectIndex,1,{title: this.todo,complete: false})
        //   this.todo = ''
      },
      completeTask(data){
          data.complete = !data.complete
      },
      allData(){
          this.todos.forEach(todo=>{
              todo.complete = true
          })
      },
      allDelete(){
          this.todos = this.todos.filter(todo=> todo.complete === false)
      }
       
  }
}
</script>
<style scoped>
.completed{
    text-decoration: line-through;
}
</style>