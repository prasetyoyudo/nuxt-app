<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col-lg-12">
        <h1 class="mb-5">USER PROFILE DETAIL</h1>
        <p>Nama : {{ user.name }}</p>
        <p>Gender : {{ user.gender }}</p>
        <p>Email : {{ user.email }}</p>
      </div>
    </div>

    <div class="row text-justify">
      <div class="col-lg-8">
        <h4 class="mb-5">Daftar Post</h4>
      </div>
      <div v-for="post in posts" :key="post[i]" class="pl-3 mb-5">
        <div class="row">
          <div class="col-lg-10">
            {{ post.title }}
          </div>
          <div class="col-lg-2 text-right">
            <p>DELETE</p>
          </div>
        </div>
        <div class="row">
          <div class="col-lg-12">
            {{ post.body }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  data() {
    return {
      user: {},
      posts: [],
    };
  },

  mounted() {
    console.log(this.$route.query);
    this.getUsersData();
    this.getUserPost();
  },

  methods: {
    getUsersData() {
      axios
        .get(`https://gorest.co.in/public/v2/users/${this.$route.query.user}`)
        .then((response) => {
          this.user = response.data;
          console.log(this.user);
        });
    },

    getUserPost() {
      axios
        .get(
          `https://gorest.co.in/public/v2/users/${this.$route.query.user}/posts`
        )
        .then((response) => {
          console.log(response);
          this.posts = response.data;
          console.log(this.posts);
        });
    },

    onOpenUserDetail() {},
  },
});
</script>
