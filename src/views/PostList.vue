<template>
  <div class="post-list">
    <div class="edit-post">
      <div class="card bg-dark mb-3" v-if="editFormDiv">
        <div class="card-body">
          <form @submit.prevent="updatePost(id)">
            <div class="form-group">
              <label>Post Title</label>
              <input
                v-model="title"
                type="text"
                name="title"
                class="form-control"
                placeholder="Enter post title"
              />
            </div>
            <div class="form-group">
              <label>Post Details</label>
              <textarea
                v-model="body"
                name="body"
                class="form-control"
                placeholder="Enter post details"
              ></textarea>
            </div>
            <div class="row">
              <div class="col-6 mx-auto">
                <button class="btn btn-block btn-success">Update Post</button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
    <div
      class="card mb-3"
      v-for="post of posts"
      :key="post.id"
      @dblclick="$emit('del-post', post.id)"
    >
      <div class="card-body">
        <h3>
          <router-link :to="`/post-details/${post.id}`">{{
            post.title
          }}</router-link>
        </h3>
        <p>{{ post.body }}</p>
        <h6>
          <button class="btn btn-primary" v-on:click="editFormShow(post.id)">
            Edit
          </button>
        </h6>
      </div>
    </div>
    <div class="card mb-3" v-if="errors && errors.length">
      <div class="card-body">
        <p v-for="(error, index) of errors" :key="index">{{ error.message }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "PostList",
  props: ["posts"],
  errors: [],

  data() {
    return {
      editFormDiv: false,
      id: null,
      title: "",
      body: "",
    };
  },
  methods: {
    editFormShow(id) {
      axios
        .get(`https://my-json-server.typicode.com/Tarzan0607/demo/posts/${id}`)
        .then((res) => {
          this.id = id;
          this.title = res.data.title;
          this.body = res.data.body;
        })
        .catch((e) => {
          this.errors.push(e);
        });
      this.editFormDiv = true;
    },
    updatePost(id) {
      const udpatePost = {
        id: id,
        title: this.title,
        body: this.body,
      };

      // Send up to parent
      this.$emit("edit-post", udpatePost);
      (this.id = null), (this.title = ""), (this.body = "");
      this.editFormDiv = false;
    },
  },
};
</script>

<style>
</style>
