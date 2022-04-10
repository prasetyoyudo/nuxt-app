<template>
  <div class="container">
    <h3 class="p-3 text-center">DAFTAR USER</h3>
    <nuxt-link class="mb-2" :to="{ path: 'add' }">Tambah User</nuxt-link>
    <table class="table table-striped table-bordered text-center">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
          <th>Gender</th>
          <th>Status</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in usersData" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.gender }}</td>
          <td>{{ user.status }}</td>
          <td>
            <nuxt-link :to="{ path: 'detail', query: { user: user.id } }"
              >VIEW</nuxt-link
            >
            |
            <nuxt-link :to="{ path: 'add', query: { user: user.id } }"
              >UPDATE</nuxt-link
            >
            |
            <span
              style="cursor: pointer; color: #007bff"
              v-on:click="onDeleteUser(user.id)"
              >DELETE</span
            >
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";

export default Vue.extend({
  data() {
    return {
      usersData: [] as any,
    };
  },

  mounted() {
    this.getUsersData();
  },

  methods: {
    getUsersData() {
      axios.get("https://gorest.co.in/public/v2/users").then((response) => {
        this.usersData = response.data;

        let session: any;
        session = sessionStorage.getItem("value");
        let updatedUser: any;
        updatedUser = sessionStorage.getItem("update");
        if (session != null) {
          this.usersData.push(JSON.parse(session));
        }

        if (updatedUser != null) {
          console.log(updatedUser)
          this.usersData.push(JSON.parse(updatedUser))

        }
      });
    },

    onOpenUserDetail() {},

    onDeleteUser(event: any) {
      axios
        .get(`https://gorest.co.in/public/v2/users/${event}`)
        .then((response) => {
          console.log(response)
          this.usersData = this.usersData.filter(function (dataUsers: any) {
            if (response != undefined) {
              if (dataUsers.id == response.data.id) {
                return false;
              } else {
                return true;
              }
            }
          });
        });
    },
  },
});
</script>
