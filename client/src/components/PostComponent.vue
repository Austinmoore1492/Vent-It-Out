<template>
  <div>
    <div class="header">
      <div class="container">
        Vent<span style="color: #42b883">It</span>Out
      </div>
    </div>
    <div class="container">
      <div class="create-post">
        <label class="vent-class" for="create-post">Vent It Out...</label>
        <input type="text" id="create-post" v-on:keyup.enter="createPost" v-model="text" placeholder="Create a post">
        <button class="btn" v-on:click="createPost">Post!</button>
      </div>
      <hr>
      <p class="error" v-if="error">{{ error }}</p>
      <p class="noPost">You need to add something to post</p>
      <div class="posts-container">
        <div class="post"
          v-for="(post, index) in posts"
          :item="post"
          :index="index"
          :key="post._id"
          v-on:dblclick="deletePost(post._id)"
        >
          <span class="created-at">
            {{ `${post.createdAt.getMonth() + 1}/${post.createdAt.getDate()}/${post.createdAt.getFullYear()}`}}
            </span>
          <p class="text">{{ post.text }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from '../PostService';

export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      text: ''
    }
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch(err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      if(this.text === ""){
        const noPost = document.querySelector('.noPost')
        noPost.style.opacity = 1
        setTimeout(() => noPost.style.opacity = 0, 3000);
      } else {
        await PostService.insertPost(this.text);
        this.posts = await PostService.getPosts();
        this.text = "";
      }
    },
    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
};
</script>

<style scoped>

.header {
  background: #35495e;
  margin: 0;
  margin-bottom: 1rem;
  font-size: 2rem;
  padding: 0.4rem;
  font-weight: bold; 
  color: #fafafa;
}
.container {
  width: 80%;
  margin: 0 auto;
  word-break: break-word;
}

.create-post-container {
  margin: 0.5rem auto;
}

.vent-class {
  /*display: inline-block;
  font-size: 1.5rem;
  text-align: center;
  font-weight: 700;*/
  display: none;
}

#create-post {
  display: block;
  text-align: center; 
  margin: 0.5rem auto;
  margin-top: 2rem; 
  background: #ddd;
  border: none;
  border-bottom: 3px solid #35495e;
  width: 75%;
  font-size: 2rem;
  padding: 0 0.5rem;
  transition: all 0.5s ease;
}

#create-post:focus {
  outline: none;
  background: #fafafa;
  border-bottom: 3px solid #42b883;
}

.btn {
  display: block;
  background-color: #35495e;
  color: #f0f0f0;
  padding: 5px;
  border: none;
  width: 25%;
  border-radius: 5px;
  margin: auto;
  margin-top: 1rem;
  font-size: 1.2rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.5s ease; 
}

.btn:hover {
  background-color: #42b883;
  color: #35495e;
}

hr {
  border: 0;
  height: 3px;
  margin: 1rem 0;
  background-image: linear-gradient(
    to right,
    #35495e,
    #42b883,
    #35495e
  );
}

.error, .noPost {
  border: 1px solid #a80004;
  background-color: #b52e31;
  color: #f0f0f0;
  padding: 5px;
  margin-bottom: 1rem;
  font-size: 1.5rem; 
}

.noPost {
  opacity: 0;
  font-size: 1rem;
}

.posts-container {
  grid-gap: 1rem;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
}

.post {
  position: relative;
  padding: 0.5rem;
  padding-bottom: 2rem;
  padding-right: 4rem;
  border: 2px solid #1d1d1d;
  background-color: #f0f0f0;
}

.created-at{
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 5px;
  color: #35495e;
  font-size: 0.8rem;
  font-weight: bold;
}

.text {
  font-size: 1.2rem;
  font-weight: 700;
  margin: 0;
}

@media(max-width: 800px) {
  .header {
    font-size: 1.5rem; 
  }
  .container {
    width: 90%;
  }
  .create-post-container {
    width: 100%; 
  }
  .posts-container {
    grid-gap: 1rem;
    display: grid;
    grid-template-columns: repeat(1, 1fr);
  }
  .post {
    padding: 0;
  }
  .created-at{
    position: static;
    display: block;
    padding-left: 0.5rem;
    background: #42b883;
  }
  .text {
    padding: 1rem 0.5rem;
  }
  #create-post {
    display: block;
    text-align: center;
    margin: 0.5rem auto;
    background: #ddd;
    border: none;
    border-bottom: 2px solid #1d1d1d;
    width: 95%;
    font-size: 1.5rem;
  }
  .vent-class {
    /*display: block; 
    font-size: 2rem;*/
    display: none; 
  }
  hr {
    border: 0;
    height: 3px;
    margin: 1rem 0;
    background-image: linear-gradient(
      to right,
      #35495e,
      #35495e,
      #35495e
    );
  }
  .btn {
    width: 40%; 
  }
  .btn:hover {
    background-color: #35495e;
    color: #f0f0f0;
}
}
</style>