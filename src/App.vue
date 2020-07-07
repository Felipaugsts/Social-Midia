<template>
  <v-app>
   

    <v-content id="app">

    <SiteNav v-if="showNav"></SiteNav>
    <router-view/>
  

    </v-content>

  </v-app>
</template>



<script>
import { mapState } from 'vuex'
import SiteNav from '@/components/SiteNav'


export default {
  components: {
    SiteNav
  },
  computed: {
    ...mapState(['userProfile']),
    showNav() {
      return Object.keys(this.userProfile).length > 1
    }
  }
}
</script>
<style> 

</style>
