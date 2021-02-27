<template>
<v-container>
    
    <v-row justify="center">
        <v-col
          cols="12"
          sm="6"
          md="6"
        >
        <v-card outlined class="pa-2">
             <v-row>
              <v-col cols="12" sm="12">
                  <v-text-field
          
            dense
            label="Todo"
            v-model="todo"
            v-on:keyup.enter="submit"
            
        
          ></v-text-field>
          

              </v-col>
              <v-col cols="12" sm="12" class="text-center">
                  <v-btn class="ma-2" outlined color="success" @click="filterTodo = 'active'" >Active</v-btn>
    <v-btn class="ma-2" outlined color="success" @click="filterTodo = 'complete'" >Complete</v-btn>
    <v-btn class="ma-2" outlined color="success" @click="filterTodo = 'allSelect'">All Data</v-btn>

              </v-col>
                  <v-col cols="12" sm="12" class="text-center">
     <v-btn v-if="!isEditing" class="ma-2" color="success" @click="completeAllData">Select All Complete</v-btn>
     <v-btn v-else class="ma-2" color="success" @click="unselectAllData">Unselected All Complete</v-btn>
             <v-btn class="ma-2" color="error" @click="allDelete">Remove All Complete</v-btn>

              </v-col>
                    <v-col cols="12" sm="12">
                        <div v-for="(todo,index) in  computedFilter" :key="index">
                <input type="checkbox" value :checked="todo.complete"  v-on:change="completeTask(todo)" class="ma-2"/>
                <span :class="{completed: todo.complete}">{{  todo.title }}</span>
                <v-spacer></v-spacer>
                <v-btn class="ma-2" color="primary" @click="edit(todo,index)">Edit</v-btn>
                <v-btn class="ma-2" color="error" @click="remove(index)">Remove</v-btn>
                <v-divider></v-divider>
            </div>


              </v-col>
          </v-row>

        </v-card>
         <v-dialog v-model="dialog" transition="dialog-bottom-transition" max-width="500">
           <v-card outlined class="pa-5">
            <v-text-field
          
            dense
            label="Todo"
            v-model="newTodo"
            v-on:keyup.enter="updateData"
            
        
          ></v-text-field>
          <v-btn class="ma-2" color="error" @click="cancel">Cancel</v-btn>
          <v-btn color="success" @click="updateData">Update</v-btn>
           </v-card>
         </v-dialog>
          
        </v-col>
       
         

    </v-row>
    
   
            <v-snackbar
            right top
          v-model="snackbar"
          :color="snackbarcolor"

          >
          {{ snackbarText }}

         <template v-slot:action="{ attrs }">
        <v-btn
          color="pink"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
            
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
     filterTodo: '',
      snackbar: false,
      text: `Successfully done!`,
      editTodo: '',
      dialog: false,
      snackbarText: '',
      snackbarcolor: ''
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
              this.snackbar = true
              this.setSnackbar('success', 'Successfully added!')
              this.todos.push({
              title: this.todo,
              complete: false
          })
          this.todo = ''

          }
          
      },
      remove(id){
          this.todos.splice(id,1)
          this.snackbar = true
          this.setSnackbar('error','Successfully removed')
      },
      edit(data,id){
          this.dialog = true
          
          this.newTodo = data.title
          this.selectIndex = id
      },
      cancel(){
      this.dialog = false
      },
      updateData(){
          this.dialog = false
          this.todos.splice(this.selectIndex,1,{title: this.newTodo,complete: false})
          this.todo = ''
          this.snackbar = true
          this.setSnackbar('success', 'Successfully updated!')
          
      },
      completeTask(data){
          data.complete = !data.complete
      },
        completeAllData(){
            this.isEditing = true
          this.todos.forEach(todo=>{
              todo.complete = true
          })
      },
      unselectAllData(){
          this.isEditing = false
          this.todos.forEach(todo=>{
              todo.complete = false
          })
      },
      allDelete(){
          this.todos = this.todos.filter(todo=> todo.complete === false)
          this.snackbar = true;
          this.setSnackbar('error','Succefully all removed')
      },
      setSnackbar(color,text){
        this.snackbarcolor = color;
        this.snackbarText = text
      },
     

       
  }
}
</script>
<style scoped>
.completed{
    text-decoration: line-through;
}
</style>