<template>
  <div class="users-show">
    <h1>My Gear</h1>
    <div v-for="gear in user.gears" v-bind:key="gear.id">
      <div class="html_to_pdf">
        <img v-if="gear.images.length >= 1" :src="gear.images[0].photo_url" />
        <p>
          Category: {{ gear.category }} Make: {{ gear.make }} Model: {{ gear.model }} Color: {{ gear.color }} Serial
          Number: {{ gear.serial_number }} Other Info: {{ gear.other_info }} Registered: {{ gear.registered }} Missing:
          {{ gear.missing }}
        </p>
        <p>Add to report?</p>
        <input type="checkbox" v-model="gear.report" value="true" />
        <br />
        <router-link :to="`/gears/${gear.id}/edit`">Edit Gear</router-link>
      </div>
    </div>
    <router-link to="/gears/new">Add Gear</router-link>
    <button v-on:click="generateReport()">Send Report</button>
  </div>
</template>

<script>
import axios from "axios";
import jsPDF from "jspdf";
import "jspdf-autotable";

export default {
  data: function () {
    return {
      user: {},
      reportItems: [],
    };
  },
  created: function () {
    axios.get(`users/${this.$route.params.id}`).then((response) => {
      console.log("User Object", response.data);
      this.user = response.data;
    });
  },
  methods: {
    addToReport: function () {},
    generateReport: function () {
      (this.reportItems = []),
        this.user.gears.forEach((gear) => {
          if (gear.report === true) {
            this.reportItems.push(gear);
          }
        });
      const columns = [
        { title: "Make", dataKey: "make" },
        { title: "Model", dataKey: "model" },
        { title: "Color", dataKey: "color" },
        { title: "Serial Number", dataKey: "serial_number" },
        { title: "Other Info", dataKey: "other_info" },
      ];
      const doc = new jsPDF({
        orientation: "portrait",
        unit: "in",
        format: "letter",
      });
      doc.setFontSize(16).text("Missing Items", 0.5, 1.0);
      doc.setLineWidth(0.01).line(0.5, 1.1, 8.0, 1.1);
      doc.autoTable({
        columns,
        body: this.reportItems,
        margin: { left: 0.5, top: 1.25 },
      });
      doc.save(`Missing Items.pdf`);
    },
  },
};
</script>
