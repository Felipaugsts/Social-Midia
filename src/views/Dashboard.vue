<template>
  <div id="dashboard">
    <transition name="fade">
      <CommentModal v-if="showCommentModal" :post="selectedPost" @close="toggleCommentModal()"></CommentModal>
    </transition>
    <section>
      <div class="col1">
        <div class="profile" >
       
          <h2>{{ userProfile.name }}</h2>
          <p class="font-weight-light">{{ userProfile.title }}</p>
        
          <div class="create-post">
           
            <form @submit.prevent class="text-right">
              <textarea placeholder="Post something ..." v-model.trim="post.content"></textarea>
            <v-btn class="ma-1 pa-5 green white--text" @click="createPost()" :disabled="post.content === ''"> Post</v-btn>
            </form>
          </div>
        </div>
      </div>
      <div id="tweets" class="col2 ml-2 mr-2">
      <v-divider class="mb-2"></v-divider>
      <h2> Tweets</h2>
        <div v-if="posts.length">
          <div v-for="post in posts" :key="post.id" class="post">
            <h5>{{ post.userName }}</h5>
            <span>{{ post.createdOn | formatDate }}</span>
            <p>{{ post.content | trimLength }}</p>
            <ul>
              
              <v-btn @click="toggleCommentModal(post)" text> <v-icon>mdi-comment</v-icon> Comments {{ post.comments }}</v-btn>
            
              <v-btn @click="likePost(post.id, post.likes)" text> <v-icon>mdi-thumb-up</v-icon>Likes {{ post.likes }}</v-btn>
        
              <v-btn @click="viewPost(post)" text> <v-icon>mdi-fullscreen</v-icon>Full post </v-btn>
            </ul>
          </div>
        </div>
        <div v-else>
          <p class="no-results">There are currently no posts</p>
        </div>
      </div>
    </section>

    <!-- full post modal -->

    <transition name="fade">
      <div v-if="showPostModal" class="p-modal">
        <div class="p-container">
          <a @click="closePostModal()" class="close"><v-icon>mdi-close</v-icon></a>
          <div class="post">
            <h2>{{ fullPost.userName }}</h2>
            <span>{{ fullPost.createdOn | formatDate }}</span>
            <p>{{ fullPost.content }}</p>
            <ul>
              <li><a><v-icon color="blue"> mdi-comment</v-icon> comments {{ fullPost.comments }}</a></li>
              <li><a><v-icon color="blue"> mdi-thumb-up</v-icon> likes {{ fullPost.likes }}</a></li>
            </ul>
           
             
          </div>
          <div v-show="postComments.length" class="comments">
            <div v-for="comment in postComments" :key="comment.id" class="comment">
              <h5>{{ comment.userName }}</h5>
              <span>{{ comment.createdOn | formatDate }}</span>
              <p>{{ comment.content }}</p>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import { commentsCollection } from '@/firebase'
import { mapState } from 'vuex'
import moment from 'moment'
import CommentModal from '@/components/CommentModal'

export default {
  components: {
    CommentModal
  },
  data() {
    return {
      post: {
        content: ''
      },
      showCommentModal: false,
      selectedPost: {},
      showPostModal: false,
      fullPost: {},
      postComments: []
    }
  },
  computed: {
    ...mapState(['userProfile', 'posts'])
  },
  methods: {
    createPost() {
      this.$store.dispatch('createPost', { content: this.post.content })
      this.post.content = ''
    },
    toggleCommentModal(post) {
      this.showCommentModal = !this.showCommentModal

      // if opening modal set selectedPost, else clear
      if (this.showCommentModal) {
        this.selectedPost = post
      } else {
        this.selectedPost = {}
      }
    },
    likePost(id, likesCount) {
      this.$store.dispatch('likePost', { id, likesCount })
    },
    async viewPost(post) {
      const docs = await commentsCollection.where('postId', '==', post.id).get()

      docs.forEach(doc => {
        let comment = doc.data()
        comment.id = doc.id
        this.postComments.push(comment)
      })

      this.fullPost = post
      this.showPostModal = true
    },
    closePostModal() {
      this.postComments = []
      this.showPostModal = false
    }
  },
  filters: {
    formatDate(val) {
      if (!val) { return '-' }

      let date = val.toDate()
      return moment(date).fromNow()
    },
    trimLength(val) {
      if (val.length < 200) { return val }
      return `${val.substring(0, 200)}...`
    }
  }
}
</script>

<style lang="scss" scoped>
#tweets {
  background: #F5F5F5;
}

</style>
