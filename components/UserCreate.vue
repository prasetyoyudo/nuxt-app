<template>
  <div class="container">
    <div class="row">
      <div class="col-lg-12 mt-5">
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
          <b-form-group
            id="input-group-1"
            label="Email address:"
            label-for="input-1"
            description="We'll never share your email with anyone else."
          >
            <b-form-input
              id="input-1"
              v-model="form.email"
              type="email"
              placeholder="Enter email"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Your Name:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="form.name"
              placeholder="Enter name"
              required
            ></b-form-input>
          </b-form-group>

          <label class="mr-sm-2" for="inline-form-custom-select-pref"
            >Gender</label
          >
          <b-form-select
            id="inline-form-custom-select-pref"
            class="mb-2 mr-sm-2 mb-sm-0"
            :options="[{ text: 'Choose...', value: null }, 'male', 'female']"
            :value="null"
            v-model="form.gender"
          ></b-form-select>

          <label class="mr-sm-2" for="inline-form-custom-select-pref"
            >Status</label
          >
          <b-form-select
            id="inline-form-custom-select-pref"
            class="mb-5 mr-sm-2 mb-sm-0"
            :options="[
              { text: 'Choose...', value: null },
              'active',
              'inactive',
            ]"
            :value="null"
            v-model="form.status"
          ></b-form-select>

          <b-button type="submit" variant="primary" class="mt-2"
            >Submit</b-button
          >
          <b-button type="reset" variant="danger" class="mt-2">Reset</b-button>
        </b-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      form: {
        email: "",
        name: "",
        gender: "",
        status: "",
      },
      show: true,
    };
  },

  mounted() {
    this.getUsersData();
  },

  methods: {
    onSubmit(event: any) {
      if (this.$route.query.user != undefined) {
        event.preventDefault();
        axios
          .put(
            `https://gorest.co.in/public/v2/users/${this.$route.query.user}`,
            {
              name: this.form.name,
              email: this.form.email,
              gender: this.form.gender,
              status: this.form.status,
            },
            {
              headers: {
                Authorization: `Bearer ${"9690fb8196780608aa119e9cadbf3901b8d6679995f2667a259a1f07fb7617cd"}`,
              },
            }
          )
          .then((response) => {
            sessionStorage.setItem("update", JSON.stringify(response.data));
            this.$router.push("/");
          });
      } else {
        event.preventDefault();
        JSON.stringify(this.form);
        axios
          .post(
            "https://gorest.co.in/public/v2/users",
            {
              name: this.form.name,
              email: this.form.email,
              gender: this.form.gender,
              status: this.form.status,
            },
            {
              headers: {
                Authorization: `Bearer ${"9690fb8196780608aa119e9cadbf3901b8d6679995f2667a259a1f07fb7617cd"}`,
              },
            }
          )
          .then((response) => {
            sessionStorage.setItem("value", JSON.stringify(response.data));
            this.$router.push("/");
          });
      }
    },

    getUsersData() {
      axios
        .get(`https://gorest.co.in/public/v2/users/${this.$route.query.user}`)
        .then((response) => {
          this.form.email = response.data.email;
          this.form.name = response.data.name;
          this.form.gender = response.data.gender;
          this.form.status = response.data.status;
        });
    },

    onReset(event: any) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.gender = "";
      this.form.status = "";
      // Trick to reset/clear native browser form validation state
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
});
</script>
