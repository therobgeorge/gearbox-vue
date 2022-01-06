<template>
  <div class="gears-new">
    <form v-on:submit.prevent="createGear()">
      <h1>Add Gear</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Category:</label>
        <input type="text" class="form-control" v-model="newGearParams.category" placeholder="Category" />
      </div>
      <div class="form-group">
        <label>Make:</label>
        <input type="text" class="form-control" v-model="newGearParams.make" placeholder="Make" />
      </div>
      <div class="form-group">
        <label>Model:</label>
        <input type="text" class="form-control" v-model="newGearParams.model" placeholder="Model" />
      </div>
      <div class="form-group">
        <label>Color:</label>
        <input type="text" class="form-control" v-model="newGearParams.color" placeholder="Color" />
      </div>
      <div class="form-group">
        <label>Serial Number:</label>
        <input type="text" class="form-control" v-model="newGearParams.serial_number" placeholder="Serial Number" />
      </div>
      <div class="form-group">
        <label>Regestered With Manufacturer:</label>
        <select name="registered" v-model="newGearParams.registered">
          <option value="true">Yes</option>
          <option value="false">No</option>
        </select>
      </div>
      <div class="form-group">
        <label>Other Identifying Information:</label>
        <textarea
          cols="30"
          rows="10"
          v-model="newGearParams.comment"
          placeholder="Does it have stickers? Scratches? Anything else?"
        ></textarea>
      </div>
      <div class="form-group">
        <label>Photo:</label>
        <input type="file" v-on:change="setFile($event)" ref="fileInput" />
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
      user: {},
      newGearParams: { user_id: `${this.currentUserId}`, registered: false },
      errors: {},
      currentUserId: "",
      photo_url: "",
      newGearId: "",
    };
  },
  created: function () {
    if (localStorage.user_id) {
      this.currentUserId = localStorage.user_id;
    }
  },
  methods: {
    createGear: function () {
      axios
        .post(`/gears`, this.newGearParams)
        .then((response) => {
          console.log(response.data);
          this.newGearId = response.data.id;
          if (this.photo_url != "") {
            this.createImage(this.newGearId);
            this.$router.push(`/users/${this.currentUserId}`);
          } else {
            this.$router.push(`/users/${this.currentUserId}`);
          }
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    setFile: function (event) {
      if (event.target.files.length > 0) {
        this.photo_url = event.target.files[0];
      }
    },
    createImage: function (gear_id) {
      let formData = new FormData();
      formData.append("gear_id", gear_id), formData.append("photo_url", this.photo_url);
      axios
        .post("/images", formData)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
      axios
        .post("/images", this.newImageParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
