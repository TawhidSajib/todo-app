<template>
<v-container>
    <v-row>
        <v-col
          cols="12"
          sm="6"
          md="4"
        >
          <v-text-field
            dense
            label="Tod0-list"
            v-model="todo"
          ></v-text-field>
          
        </v-col>
         <v-btn color="success" v-if="!isEditing" @click="submit">Add</v-btn>
         <v-btn color="primary" v-else @click="update">Update</v-btn>
         

    </v-row> 
    <ol>
    <li v-for="(todo,index) in todos" :key="index">
       <input type="checkbox" v-on:change="completeTask(todo)" v-bind:checked="todo.isComplete"/>
       <span v-bind:class="{completed: todo.isComplete}">{{todo.title}}</span>
      <v-btn color="primary" style="margin-right: 3px;margin-left: 3px;" @click="edit(todo,index)">Edit</v-btn>
      <v-btn color="error" style="margin-left: 3px;" @click="remove(index)">Delete</v-btn>
    </li>  
    </ol>             
</v-container>
  
</template>
<script>
export default {
  data(){
    return {
      
      isEditing: false,
      todo: '',
      todos: [{title: 'Go home',isComplete: false},{title: 'Pack bag',isComplete: false},{title: 'Go for hair cut',isComplete: false}],
      selectId: '',
      completed: true
    }
  },
 
  methods: {
       submit(){
         this.todos.push({title: this.todo,isComplete: false})
         this.todo = ''
       },
       remove(id){
         this.todos.splice(id,1)
       },
       edit(data,id){
         this.isEditing = true
         this.todo = data.title
         this.selectId = id

       },
       update(){
         this.isEditing = false
         this.todos.splice(this.selectId,1,this.todo)
         this.todo = ''
       },
       completeTask(todo){
         todo.isComplete = !todo.isComplete
       },
     
       
  }
}
</script>
<style scoped>
.completed{
  text-decoration: line-through;
}
</style>