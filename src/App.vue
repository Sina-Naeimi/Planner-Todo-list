<template>
  <v-app id="inspire">
   <AppBar/>
    <v-main>
      <router-view></router-view>
    </v-main>
    <Footer/>
  </v-app>
</template>

<script>
import AppBar from './components/AppBar.vue'
import Footer from './components/Footer.vue'
export default {
  data: () => ({
   
  }),
  components:{
    AppBar,
    Footer
  },
  methods:{
 
  }

};
</script>