<template>
  <div class="home">
    <AddPost v-on:add-post="addPost" />
    <PostList
      v-on:del-post="deletePost"
      v-on:edit-post="updatePost"
      v-bind:posts="posts"
    />
  </div>
</template>

<script>
import axios from "axios";

// @ is an alias to /src
import AddPost from "@/views/AddPost.vue";
import PostList from "@/views/PostList.vue";

export default {
  name: "home",
  components: {
    AddPost,
    PostList,
  },
  data() {
    return {
      posts: [],
      errors: [],
    };
  },
  methods: {
    addPost(newPost) {
      const { title, body } = newPost;
      alert("ssssss");
      axios
        .post("https://my-json-server.typicode.com/Tarzan0607/demo/posts", {
          title,
          body,
        })
        .then((res) => (this.posts = [...this.posts, res.data]))
        .catch((err) => console.log(err));
    },
    updatePost(updatePost) {
      this.posts = this.posts.map((post) => {
        if (updatePost.id == post.id) {
          return updatePost;
        } else {
          return post;
        }
      });
    },
    deletePost(id) {
      axios
        .delete(
          `https://my-json-server.typicode.com/Tarzan0607/demo/posts/${id}`
        )
        .then((res) => {
          this.posts = this.posts.filter((post) => post.id !== id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  created() {
    axios
      .get(`https://my-json-server.typicode.com/Tarzan0607/demo/posts?_limit=5`)
      .then((res) => {
        this.posts = res.data;
      })
      .catch((e) => {
        this.errors.push(e);
      });
  },
};
</script>
