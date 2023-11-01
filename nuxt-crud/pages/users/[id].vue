<template lang="">
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h4>
          Edit User
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
  name: "userEdit",
  data() {
    return {
      userId: "",
      user: {},
      isLoading: false,
      isLoadingText: "Loading",
      errorList: {},
    };
  },
  mounted() {
    this.userId = this.$route.params.id;
    this.getUser(this.userId);
  },
  methods: {
    getUser(userId) {
      axios.get(`http://localhost:8000/api/users/${userId}`).then((res) => {
        this.user = res.data.data;
        console.log(this.user);
      });
    },
    saveUsers() {
      this.isLoading = true;
      this.isLoadingText = "Saving";

      let myThis = this;
      axios
        .put(`http://localhost:8000/api/users/${this.userId}`, this.user)
        .then((res) => {
          // this.user.name = "";
          // this.user.email = "";

          console.log(res);
          alert("User updated!");
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
