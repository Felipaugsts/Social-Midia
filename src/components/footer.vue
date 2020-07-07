<template>
  <v-footer class="text-center white">
    
  
   <v-btn
          v-for="icon in icons"
          :key="icon.icon"
          class="mx-4"
          :href="icon.url"
          dark
          icon
        >
          <v-icon color="black" size="20px">{{ icon.icon }}</v-icon>
        </v-btn>
  </v-footer>
</template>
        

       


<script>
  export default {
    data: () => ({
      icons: [
        
       { icon: 'mdi-web', url: "https://phill-vue.web.app/" },
     {   icon: 'mdi-linkedin',  url: "https://www.linkedin.com/in/felipe-augusto-091b8917a/ " }
   
      ],
    }),
  }
</script>
<style> 


 </style>