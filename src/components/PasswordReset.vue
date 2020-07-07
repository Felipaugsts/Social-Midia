<template>
  <div class="modal">
    <div class="modal-content">
      <div @click="$emit('close')" class="close"><v-icon> mdi-close</v-icon></div>
      <h3>Reset Password</h3>
      <div v-if="!showSuccess">
        <p>Enter your email to reset your password</p>
        <form @submit.prevent>
          <v-text-field v-model.trim="email" type="email" placeholder="example@gmail.com" />

        </form>
        <p v-if="errorMsg !== ''" class="error">{{ errorMsg }}</p>
        <v-btn class="primary" @click="resetPassword()">Reset</v-btn>
      </div>
      <p class="green--text" v-else>Success! Check your email for a reset link.</p>
    </div>
  </div>
</template>

<script>
import { auth } from '@/firebase'

export default {
  data() {
    return {
      email: '',
      showSuccess: false,
      errorMsg: ''
    }
  },
  methods: {
    async resetPassword() {
      this.errorMsg = ''

      try {
        await auth.sendPasswordResetEmail(this.email)
        this.showSuccess = true
      } catch (err) {
        this.errorMsg = err.message
      }
    }
  }
}
</script>
