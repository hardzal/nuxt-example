<template lang="">
  <div class="mt-5 container">
    <h2>Create A New Post</h2>
    <div class="card">
      <div class="card-header">
        <h4>
          Add User
          <NuxtLink to="/users" class="btn btn-danger float-end">Back</NuxtLink>
        </h4>
      </div>
      <div class="card-body">
        <div v-if="isLoading">
          <Loading :title="isLoadingText" />
        </div>
        <div v-else>
          <form @submit.prevent="saveUsers">
            <div class="mb-3">
              <label>Name</label>
              <input type="text" class="form-control" v-model="user.name" />
              <span class="text-danger" v-if="this.errorList.name">{{
                this.errorList.name[0]
              }}</span>
            </div>
            <div class="mb-3">
              <label>Email</label>
              <input type="email" class="form-control" v-model="user.email" />
              <span class="text-danger" v-if="this.errorList.email">{{
                this.errorList.email[0]
              }}</span>
            </div>
            <div class="mb-3">
              <label>Password</label>
              <input
                type="password"
                class="form-control"
                v-model="user.password"
              />
            </div>
            <span class="text-danger" v-if="this.errorList.password">{{
              this.errorList.password[0]
            }}</span>
            <div class="mb-3">
              <button type="submit" class="btn btn-primary">Save</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "userCreate",
  data() {
    return {
      user: {
        name: "",
        email: "",
        password: "",
      },
      isLoading: false,
      isLoadingText: "Loading",
      errorList: {},
    };
  },

  methods: {
    saveUsers() {
      this.isLoading = true;
      this.isLoadingText = "Saving";

      let myThis = this;
      axios
        .post("http://localhost:8000/api/users", this.user)
        .then((res) => {
          this.user.name = "";
          this.user.email = "";
          this.user.password = "";

          console.log(res);
          alert(res.data.message);
          this.isLoading = false;
          this.isLoadingText = "Loading";
        })
        .catch(function (error) {
          if (error.response?.status === 422) {
            myThis.errorList = error.response.data.errors;
          }

          myThis.isLoading = false;
        });
    },
  },
};
</script>
