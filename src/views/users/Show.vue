<template>
  <div class="users-show">
    <h1>My Gear</h1>
    <div v-for="gear in user.gears" v-bind:key="gear.id">
      <div class="html_to_pdf">
        <img v-if="gear.images.length >= 1" :src="gear.images[0].photo_url" />
        <p>
          {{ gear.category }} {{ gear.make }} {{ gear.model }} {{ gear.color }} {{ gear.serial_number }}
          {{ gear.other_info }} {{ gear.registered }} {{ gear.missing }}
        </p>
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
import html2canvas from "html2canvas";

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
  methods: {
    generateReport: function () {
      window.html2canvas = html2canvas;
      let doc = new jsPDF();
      doc.html(document.querySelector(".html_to_pdf"), {
        callback: function (pdf) {
          pdf.save("missingitems.pdf");
        },
      });
    },
  },
};
</script>
