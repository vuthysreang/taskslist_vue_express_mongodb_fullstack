<template>
  <div class="container">
    <h1>Latest tasks</h1>
    <div class="create-post">
      <label for="create-post">Post task: </label>
      <input type="text" id="create-post" v-model="task" placeholder="Create a task">
      <button v-on:click="createPost">Submit</button>
    </div>
    <hr>
    <p class="error" v-if="error">{{error}}</p>
    <div class="posts-container">
      <div class="post"
        v-for="(post, index) in posts"
        :item = "post"
        :index = "index"
        :key = "post._id"
        v-on:dblclick="deletePost(post._id)"
      >
      {{`${post.dateCreated.getDate()}/${post.dateCreated.getMonth() + 1}/${post.dateCreated.getFullYear()}`}}
      <p class="text">{{post.task}}</p>

      </div>
    </div>
  </div>
</template>

<script>
import PostService from '../PostService'

export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      task: '',
    }
  },
  async created(){
    try{
      this.posts = await PostService.getPosts();
    }catch(err) {
      this.error = err.message;
    }
  },
  methods:{
    async createPost(){
      await PostService.insertPost(this.task)
      this.task = ''
      this.posts = await PostService.getPosts();
      
    },
    async deletePost(id){
      await PostService.deletePost(id)
      this.posts = await PostService.getPosts();
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.container {
  max-width: 800px;
  margin: 0 auto;
}
input{
  width: 30%;
  padding: 5px 10px;
}
button{
  cursor: pointer;
  background-color: #1eccc3;
  margin-left: 5px;
  padding: 4px 9px;
}
p.error {
  border: 1px solid #ff5b5f;
  background-color: #ffc5c1;
  padding: 10px;
  margin-bottom: 15px;
}

div.post {
  position: relative;
  border: 3px solid black;
  background-color: #bcffb8;
  padding: 10px 10px 30px 10px;
  margin-bottom: 15px;
}

div.created-at {
  position: absolute;
  top: 0;
  left: 0;
  padding: 5px 15px 5px 15px;
  background-color: darkgreen;
  color: white;
  font-size: 13px;
}

p.text {
  font-size: 22px;
  font-weight: 700;
  margin-bottom: 0;
}
</style>
