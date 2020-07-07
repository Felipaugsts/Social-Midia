<template>
  <header>
    <section id="navv">
      <div id="large" class="col1">
        <router-link to="/">
          <h3>Social App</h3>
        </router-link>
        <ul class="inline">
   
          <v-btn to="/"> <v-icon> mdi-home</v-icon> Home</v-btn>
        
          
             <v-btn  to="/settings" class="ml-2 mr-2"> <v-icon class="ml-1"> mdi-edit</v-icon> Settings</v-btn>
         
          <v-btn @click="logout()" > Log out</v-btn>
        </ul>
      </div>

<div id="hamburger" class="ml-8"> 
    <v-btn @click="close = true, menu = false, drawer = true" v-show="menu" large icon><v-icon large color="black">mdi-menu</v-icon></v-btn>
     <v-btn @click="close = false, menu = true, drawer = false"  v-show="close" large icon><v-icon large color="black">mdi-close</v-icon></v-btn>
</div>

<div v-show="drawer" id="drawer"> 
<v-btn @click="close = false, menu = true, drawer = false" to="/" text class="mt-12"> Home</v-btn>
<v-btn @click="close = false, menu = true, drawer = false" to="settings" text class="mt-5"> Settings</v-btn>
<v-btn @click="logout(), drawer = false" text class="mt-5">  Log out</v-btn>

<v-divider class="mt-5"></v-divider>

<div class="text-center mt-10">
<v-btn  
href="https://phill-vue.web.app"
 icon><v-icon color="blue">mdi-web</v-icon></v-btn>

</div>
</div>
  
    </section>
  </header>
</template>

<script>
export default {
  data() {
    return {
menu: true,
close: false,
drawer:false
  }},
  methods: {
    logout() {
      this.$store.dispatch('logout')
    }
  }
}
</script>
<style> 
@media screen and (max-width: 742px) {
#large {
  display: none;
}
  #navv {

   background:transparent;
   position: fixed;

}
}

#drawer { 
  height: 50vh;
  width: 50vh;
  background-color: #F5F5F5;
  display: flex;
  flex: flex;
flex-direction: column;
border-radius: 0px 0px 30px 0px;

}
  @media screen and (min-width: 742px) {
      #hamburger { 
        display: none;
      }
    
    }
</style>