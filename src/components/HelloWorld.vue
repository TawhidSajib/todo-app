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
      {{ todo }}
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
      todos: [],
      selectId: ''
    }
  },
  methods: {
       submit(){
         this.todos.push(this.todo)
         this.todo = ''
       },
       remove(id){
         this.todos.splice(id,1)
       },
       edit(data,id){
         this.isEditing = true
         this.todo = data
         this.selectId = id

       },
       update(){
         this.isEditing = false
         this.todos.splice(this.selectId,1,this.todo)
         this.todo = ''
       }
  }
}
</script>