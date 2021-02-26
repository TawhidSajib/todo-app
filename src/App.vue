<template>
 <v-app id="inspire">
  

    <v-main dark>
      <newTodo/>
    </v-main>
 </v-app>
  
</template>

<script>

import newTodo from './components/newTodo';

export default {
  name: 'App',

  components: {
   
    newTodo
  },

  data: () => ({
    //
  }),
};
</script>
