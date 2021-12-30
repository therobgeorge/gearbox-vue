<template>
  <div class="users-show">
    <h1>My Gear</h1>
    <div v-for="gear in user.gears" v-bind:key="gear.id">
      <img :src="gear.images[0].photo_url" />
      <p>
        {{ gear.category }} {{ gear.make }} {{ gear.model }} {{ gear.color }} {{ gear.serial_number }}
        {{ gear.other_info }} {{ gear.registered }} {{ gear.missing }}
      </p>
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
