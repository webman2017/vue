<template>
  <select class="form-select">
    <option v-for="post in posts" :key="post.project_id" value='{{ post.project_id }}'> {{ post.projectName }}</option>
  </select>
</template>
<script>
import axios from 'axios'

export default {
  data() {
    return {
      posts: []
    }
  },
  mounted() {
    axios
      .get('https://hrd-md.aitproject.com/api/getProject/?fontendName=berm')
      .then((response) => {
        console.log(response)
        this.posts = response.data.project
      })
  }
}
</script>