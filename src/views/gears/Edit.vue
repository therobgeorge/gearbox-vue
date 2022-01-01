<template>
  <div class="gears-edit">
    <form v-on:submit.prevent="updateGear()">
      <h1>Edit Gear</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Category:</label>
        <input type="text" class="form-control" v-model="editGearParams.category" placeholder="Category" />
      </div>
      <div class="form-group">
        <label>Make:</label>
        <input type="text" class="form-control" v-model="editGearParams.make" placeholder="Make" />
      </div>
      <div class="form-group">
        <label>Model:</label>
        <input type="text" class="form-control" v-model="editGearParams.model" placeholder="Model" />
      </div>
      <div class="form-group">
        <label>Color:</label>
        <input type="text" class="form-control" v-model="editGearParams.color" placeholder="Color" />
      </div>
      <div class="form-group">
        <label>Serial Number:</label>
        <input type="text" class="form-control" v-model="editGearParams.serial_number" placeholder="Serial Number" />
      </div>
      <div class="form-group">
        <label>Regestered With Manufacturer:</label>
        <select name="registered" v-model="editGearParams.registered">
          <option value="true">Yes</option>
          <option value="false">No</option>
        </select>
      </div>
      <div class="form-group">
        <label>Missing:</label>
        <select name="missing" v-model="editGearParams.missing">
          <option value="true">Yes</option>
          <option value="false">No</option>
        </select>
      </div>
      <div class="form-group">
        <label>Other Identifying Information:</label>
        <textarea
          cols="30"
          rows="10"
          v-model="editGearParams.other_info"
          placeholder="Does it have stickers? Scratches? Anything else?"
        ></textarea>
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    <button v-on:click="destroyGear()">Delete Gear</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      editGearParams: {},
      errors: {},
    };
  },
  created: function () {
    axios.get(`gears/${this.$route.params.id}`).then((response) => {
      console.log("Gear Object", response.data);
      this.editGearParams = response.data;
    });
  },
  methods: {
    updateGear: function () {
      axios.patch(`gears/${this.editGearParams.id}`, this.editGearParams).then((response) => {
        console.log("Edit User", response.data);
        this.$router.push(`/users/${this.editGearParams.user_id}`);
      });
    },
    destroyGear: function () {
      if (confirm("Confirm Delete Gear"))
        axios.delete(`gears/${this.editGearParams.id}`).then((response) => {
          console.log(response.data);
          this.$router.push(`/users/${this.editGearParams.user_id}`);
        });
    },
  },
};
</script>
