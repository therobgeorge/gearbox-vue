<template>
  <div class="gears-edit">
    <h1>Edit Gear</h1>
    <ul>
      <li class="text-danger" v-for="error in errors" v-bind:key="error">
        {{ error }}
      </li>
    </ul>
    <div v-for="image in editGearParams.images" v-bind:key="image.id">
      <img :src="image.photo_url" />
      <button v-on:click="destroyImage(image)">Delete Photo</button>
    </div>
    <input type="file" v-on:change="setFile($event)" ref="fileInput" />
    <button v-on:click="createImage()">Add Photo</button>
    <form v-on:submit.prevent="updateGear()">
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
      newImageParams: {},
      photo_url: "",
    };
  },
  created: function () {
    this.setEditGearParams();
  },
  methods: {
    setFile: function (event) {
      if (event.target.files.length > 0) {
        this.photo_url = event.target.files[0];
      }
    },
    setEditGearParams: function () {
      axios.get(`gears/${this.$route.params.id}`).then((response) => {
        console.log("Gear Object", response.data);
        this.editGearParams = response.data;
      });
    },
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
    createImage: function () {
      let formData = new FormData();
      formData.append("gear_id", this.editGearParams.id), formData.append("photo_url", this.photo_url);
      axios
        .post("/images", formData)
        .then((response) => {
          console.log(response.data);
          this.editGearParams.images.push(response.data);
          this.photo_url = "";
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyImage: function (image) {
      if (confirm("Confirm Delete Image?")) {
        axios.delete(`/images/${image.id}`).then((response) => {
          console.log(response.data);
          for (var i = 0; i < this.editGearParams.images.length; i++) {
            if (this.editGearParams.images[i] === image) {
              this.editGearParams.images.splice(i, 1);
            }
          }
        });
      }
    },
  },
};
</script>
