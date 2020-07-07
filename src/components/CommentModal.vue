<template>
  <div class="c-modal">
    <div class="c-container">
      <a @click="$emit('close')"><v-icon>mdi-close</v-icon></a>
      <p>Add a comment </p>
      <form @submit.prevent>
        <textarea v-model.trim="comment"></textarea>
        <v-btn class="green white--text" @click="addComment()" :disabled="comment == ''" >add comment</v-btn>
      </form>
    </div>
  </div>
</template>

<script>
import { commentsCollection, postsCollection, auth } from '@/firebase'

export default {
  props: ['post'],
  data() {
    return {
      comment: ''
    }
  },
  methods: {
    async addComment() {
      // create comment
      await commentsCollection.add({
        createdOn: new Date(),
        content: this.comment,
        postId: this.post.id,
        userId: auth.currentUser.uid,
        userName: this.$store.state.userProfile.name
      })

      // update comment count on post
      await postsCollection.doc(this.post.id).update({
        comments: parseInt(this.post.comments) + 1
      })

      // close modal
      this.$emit('close')
    }
  }
}
</script>
