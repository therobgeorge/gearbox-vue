<template>
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <div class="form-group">
        <label>Name:</label>
        <input type="text" class="form-control" v-model="newUserParams.name" placeholder="Name" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" class="form-control" v-model="newUserParams.email" placeholder="Email Address" />
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" class="form-control" v-model="newUserParams.password" placeholder="Password" />
      </div>
      <div class="form-group">
        <label>Confrim Password:</label>
        <input
          type="password"
          class="form-control"
          v-model="newUserParams.password_confirmation"
          placeholder="Password"
        />
      </div>
      <div class="form-group">
        <label>Phone Number (optional):</label>
        <input type="text" class="form-control" v-model="newUserParams.phone_number" placeholder="Phone Number" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: {},
      errors: [],
    };
  },

  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
