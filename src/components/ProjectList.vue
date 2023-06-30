<template>
  <div
    v-for="post in posts" :key="post.project_id" value="{{ post.project_id }}"
  >
    <div>{{ post.projectName }}</div>
    <button v-on:click="handleClick" class="btn btn-warning">แก้ไข</button>
  </div>
  <div>เพิ่มโครงการ</div>
  <div id="app">
    <form @submit.prevent="submitForm">
      <div>
        <label for="projectCode">รหัสโครงการ:</label><br />
        <input id="projectCode" type="text" v-model="projectCode" required class="form-control"/>
      </div>
      <div>
        <label for="projectName">ชื่อโครงการ:</label><br />
        <input id="projectName" type="text" v-model="projectName" required  class="form-control"/>
      </div>
      <div>
        <label for="startDate">วันที่เริ่ม:</label><br />
        <input id="startDate" type="text" v-model="startDate" required  class="form-control"/>
      </div>
      <div>
        <label for="endDate">วันที่สิ้นสุด:</label><br />
        <input id="endDate" type="text" v-model="endDate" required  class="form-control"/>
      </div>
      <div>
        <label for="warranty">จำนวนวันรับประกันงาน:</label><br />
        <input id="warranty" type="text" v-model="warranty" required  class="form-control"/>
      </div>
      <div>
        <label for="description">รายละเอียดโครงการ</label><br />
        <textarea id="description" v-model="description" required  class="form-control"></textarea>
      </div>
      <button :class="[name ? activeClass : '']" type="submit" class="btn btn-success">บันทึก</button>
      <!-- <div>
        <h3>Data retrieved from server:</h3>
        <p v-if="success">{{ success }}</p>
        <pre>{{ response }}</pre>
      </div> -->
    </form>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      posts: [],
    };
  },
  methods: {
    submitForm() {
      axios
        .post(
          "https://hrd-md.aitproject.com/api/createProject?fontendName=berm",
          {
            projectCode: this.projectCode,
            projectName: this.projectName,
            startDate: this.startDate,
            endDate: this.endDate,
            warantyTotal: this.warranty,
            description: this.description,
            projectOwner: "ชื่อเจ้าของโครงการ",
          }
        )
        .then((response) => {
        //   console.log(response.data);
          // this.response = response.data
          //   this.success = "Data saved successfully";
          this.response = JSON.stringify(response.data.message, null, 2);
        })
        .catch((error) => {
          this.response = "Error: " + error.response.status;
        });
      this.name = "";
      this.email = "";
      this.firstSon = "";
    },

    handleClick() {
      axios
        .get("https://hrd-md.aitproject.com/api/getProject/?fontendName=berm")
        .then((response) => {
          console.log(response);
          // this.posts = response.data.project;
        });
    },
    handleInput() {
      console.log("Text input changed");
    },
  },
  mounted() {
    axios
      .get("https://hrd-md.aitproject.com/api/getProject/?fontendName=berm")
      .then((response) => {
        console.log(response);
      });
  },
};
</script>
