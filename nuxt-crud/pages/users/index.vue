<template lang="">
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h3>
          Welcome to user list page
          <NuxtLink to="/users/create" class="btn btn-primary float-end"
            >Create user</NuxtLink
          >
        </h3>
      </div>
      <div class="card-body">
        <div v-if="isLoading">
          <Loading title="Loading..." />
        </div>
        <div v-else>
          <table class="table table-striped table-bordered">
            <thead>
              <tr>
                <th>No</th>
                <th>Name</th>
                <th>Email</th>
                <th>Action</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(user, index) in users" :key="index">
                <td>{{ user.id }}</td>
                <td>{{ user.name }}</td>
                <td>{{ user.email }}</td>
                <td>
                  <NuxtLink
                    :to="`/users/${user.id}`"
                    class="btn btn-success btn-sm mx-2"
                    >Edit</NuxtLink
                  >
                  <button
                    type="button"
                    @click="deleteUser($event, user.id)"
                    class="btn btn-danger btn-sm mx-2"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "userList",
  data() {
    return {
      users: {},
      isLoading: true,
    };
  },
  mounted() {
    this.getUsers();
  },
  methods: {
    getUsers() {
      this.isLoading = true;

      axios.get("http://localhost:8000/api/users").then((res) => {
        console.log(res.data.data.data);
        this.isLoading = false;
        this.users = res.data.data.data;
      });
    },

    deleteUser(event, userId) {
      if (confirm("Are you sure deleting this data?")) {
        event.target.innerText = "Deleting";
        axios
          .delete(`http://localhost:8000/api/users/${userId}`)
          .then((res) => {
            event.target.innerText = "Delete";
            this.getUsers();
          });
      }
    },
  },
};
</script>
