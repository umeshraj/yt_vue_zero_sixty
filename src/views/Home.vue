<template>
  <div>
    <div class="row">
      <div class="col s6">
        <!-- form -->
        <PostForm @postCreated="addPost" />
      </div>
    </div>
    <div class="row">
      <div
        class="col s6"
        v-for="(post, index) in posts"
        v-bind:item="post"
        :index="index"
        :key="post.id"
      >
        <div class="card">
          <div class="card-content">
            <p class="card-title">{{post.title}}</p>
            <p class="timestamp">{{post.createdAt}}</p>
            <p>{{post.body}}</p>
          </div>
          <div class="card-action">
            <a href="#" @click="editPost(post)">Edit</a>
            <a href="#" class="delete-btn" @click="deletePost(post.id)">Delete</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from "../PostService";
import PostForm from "../components/PostForm.vue";
const postService = new PostService();
export default {
  name: "Home",
  components: {
    PostForm
  },
  data() {
    return {
      posts: []
    };
  },
  methods: {
    addPost(post) {
      this.posts.unshift(post);
    },
    editPost(post) {
      console.log(post);
    },
    deletePost(id) {
      postService
        .deletePost(id)
        .then(() => {
          this.posts = this.posts.filter(p => p.id !== id);
        })
        .catch(err => console.log(err));
    }
  },
  created() {
    postService
      .getAllPosts()
      .then(res => {
        this.posts = res.data;
        console.log(this.posts);
      })
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.card .card-content .card-title {
  margin-bottom: 0;
}
.card .card-content p.timestamp {
  color: #999;
  margin-bottom: 10px;
}
.delete-btn {
  color: red !important;
}
</style>