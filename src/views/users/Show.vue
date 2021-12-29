<template>
  <div class="users-show">
    <div v-for="gear in user.gears" v-bind:key="gear.id">
      {{ gear }}
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      user: {},
    };
  },
  created: function () {
    axios.get(`users/${this.$route.params.id}`).then((response) => {
      console.log("User Object", response.data);
      this.user = response.data;
    });
  },
  methods: {},
};
</script>
