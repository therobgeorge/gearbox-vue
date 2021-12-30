<template>
  <div class="users-edit">
    <form v-on:submit.prevent="updateUser()">
      <h1>Edit My Info</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Name:</label>
        <input type="text" class="form-control" v-model="editUserParams.name" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" class="form-control" v-model="editUserParams.email" />
      </div>
      <div class="form-group">
        <label>Phone Number:</label>
        <input type="text" class="form-control" v-model="editUserParams.phone_number" />
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" class="form-control" v-model="editUserParams.password" />
      </div>
      <div class="form-group">
        <label>Password Confirmation:</label>
        <input type="password" class="form-control" v-model="editUserParams.password_confirmation" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
      <button v-on:click="destroyUser()">Delete Profile</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      user: {},
      editUserParams: {},
      errors: {},
    };
  },
  created: function () {
    axios.get(`users/${this.$route.params.id}`).then((response) => {
      console.log("User Object", response.data);
      this.editUserParams = response.data;
    });
  },
  methods: {
    updateUser: function () {
      axios.patch(`users/${this.editUserParams.id}`, this.editUserParams).then((response) => {
        console.log("Edit User", response.data);
        this.$router.push(`/users/${response.data.id}`);
      });
    },
    destroyUser: function () {
      if (confirm("Confirm Delete Your Profile"))
        axios.delete(`users/${this.editUserParams.id}`).then((response) => {
          console.log(response.data);
          delete axios.defaults.headers.common["Authorization"];
          localStorage.removeItem("jwt");
          localStorage.removeItem("user_id");
          this.$router.push("/");
        });
    },
  },
};
</script>
