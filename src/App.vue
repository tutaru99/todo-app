<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
       
      </v-toolbar-title>
      <v-spacer></v-spacer>
     
    </v-toolbar>
    <v-content>

      <todo />
      
    </v-content>
  </v-app>
</template>

<script>
import todoapp from './components/todoapp.vue'

export default {
  name: 'App',
  components: {
    'todo':todoapp
  },
  data () {
    return {
      //
    }
  }
}
</script>

<style>
#app{
font-size: 21px; }

</style>
