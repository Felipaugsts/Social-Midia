<template>
  <div id="login">
    <PasswordReset  id="reset" v-if="showPasswordReset" @close="togglePasswordReset()"></PasswordReset>
    <section>
      <div id="text" class="col1 text-center mr-5 ml-5">
        <h1>Social app</h1>
<p> This is a Vue.js & Firebase project, it was inspired on Twitter  <br> 
   You can tweet, like and comment other users posts </p>
          <v-btn outlined  class="black--text mt-5 white"
          @click="toggleForm()"> Check it out</v-btn>
      </div>
      <div :class="{ 'signup-form': !showLoginForm }" class="col2">



        <form 
        v-if="showLoginForm" 
        @submit.prevent>

<v-container class=" text-center mb-5">

<v-img  id="myimg" src="../assets/twitter.png" height="50px" width="50px"/>
<div id="desapear">
<h2> Social app</h2>
<h6> This website was created to test some Firebase features</h6>
</div>
</v-container>


        <div id="loginForm"> 
          <h1>Login</h1>
      
           
         
          <v-text-field
          v-model.trim="loginForm.email"
          type="text"
           hint="Example@gmail.com"
           label="Email adress"
            />

            <v-text-field
         v-model.trim="loginForm.password"
          type="password"
           label="Password"
            />
 


          <v-btn class="green white--text" @click="loader = 'loading',login()"
         
      :loading="loading"
      :disabled="loading"
      color="green"> Login</v-btn>
          <div class="mt-5 text-right">
            
            <v-btn class="mr-3 mb-3" @click="togglePasswordReset()"> Forgot Password</v-btn>
             <v-btn class=" mb-3 mr-3" @click="toggleForm()"> Register</v-btn>
            
          </div>
          </div>
        
        </form>
       

        <form v-else @submit.prevent>
         <div id="loginForm"> 
          <h1>Get Started</h1>
          
         
          
            <v-text-field
        v-model.trim="signupForm.name"
          type="text"
           label="First and last name"
            />
          
           <v-text-field
        v-model.trim="signupForm.title"  
          type="text"
           label="Company"
            />
           <v-text-field
        v-model.trim="signupForm.email"
          type="email"
           label="Email address"
            />
          <v-text-field
        v-model.trim="signupForm.password"
          type="password"
           label="Password"
           hint="has to be at least 6 characters + 1 number"
            />
              <v-btn class="green white--text mb-3" @click="loader = 'loading',signup()"
         
      :loading="loading"
      :disabled="loading"
      color="green"> Sign up</v-btn>
       
          <div class="extras">

          

            <v-btn
             text @click="toggleForm()"> Login</v-btn>

         
          </div>
          
         </div>
        </form>
      </div>
      <v-content id="foot"> 
         <Footer />
      </v-content>
    
    </section>
  </div>
</template>

<script>
import PasswordReset from '@/components/PasswordReset'
import Footer from '@/components/footer.vue'
export default {
  components: {
    PasswordReset,
    Footer
  },
  data() {
    return {
      loginForm: {
        email: '',
        password: ''
      },
      signupForm: {
        name: '',
        title: '',
        email: '',
        password: ''
      },
      showLoginForm: true,
      showPasswordReset: false,
      loader: null,
        loading: false,
        loading2: false,
        loading3: false,
        loading4: false,
        loading5: false
    }
  },
   watch: {
      loader () {
        const l = this.loader
        this[l] = !this[l]

        setTimeout(() => (this[l] = false), 3000)

        this.loader = null
      }
   },
  methods: {
    toggleForm() {
      this.showLoginForm = !this.showLoginForm
    },
    togglePasswordReset() {
      this.showPasswordReset = !this.showPasswordReset
    },
    login() {
      this.$store.dispatch('login', {
        email: this.loginForm.email,
        password: this.loginForm.password
      })
    },
    signup() {
      this.$store.dispatch('signup', {
        email: this.signupForm.email,
        password: this.signupForm.password,
        name: this.signupForm.name,
        title: this.signupForm.title
      })
    }
  }
}
</script>
<style scoped>
#loginForm { 

  border: 1px solid gray;
  height: auto;
padding-top: 10px;
  padding-left: 20px;
  padding-right: 20px;
  border-radius: 20px;
}
#reset {
  z-index: 2;
}
#myimg {
  left: 45%;
}
#text { 
  margin-top: 17vh;
}
 @media screen and (min-width: 742px) {
   #desapear, #foot { 
     display: none;
    
   }
   }

</style>>
