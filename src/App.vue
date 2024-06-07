<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      base_api_url: 'http://127.0.0.1:8000',
      projects_endpoint: '/api/projects',
      projects: '',
    }
  },
  mounted() {
    const url = this.base_api_url + this.projects_endpoint;
    console.log(url);
    axios
      .get(url)
      .then(response => {
        console.log(response);
        this.projects = response.data.results;
      })
      .catch(error => {
        console.error(error);
      });
  }
}
</script>

<template>
  <h1 class="my-2">Projects:</h1>

  <div class="p-5 mb-4 bg-light rounded-3">
    <div class="container-fluid py-5">
      <h1 class="display-5 fw-bold">Blog</h1>
      <p class="col-md-8 fs-4">
        Read our amazing blog
      </p>

      <div class="input-group mb-3">
        <input type="search" class="form-control" placeholder="search...">
        <button class="btn btn-outline-secondary" type="button">
          <i class="fas fa-search fa-lg fa-fw"></i>
        </button>
      </div>


    </div>
  </div>


  <section v-if="projects">
    <div class="container">
      <div class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 g-5">
        <div class="col" v-for="project in projects.data">
          <div class="card" style="height: 450px;">

            <template v-if="!project.cover_image.startsWith('https://')">
              <img :src="base_api_url + '/storage/' + project.cover_image" :alt="project.title" style="height: 80%;">
            </template>
            <template v-else>
              <img :src="project.cover_image" :alt="project.title" style="height: 80%;">
            </template>

            <div class="card-body">
              {{ project.description }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

</template>

<style scoped>
div.card-body {
  overflow-y: scroll;
}
</style>
