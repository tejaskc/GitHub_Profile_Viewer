<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import { useRoute } from "vue-router";

const route = useRoute();
const userprofile = ref({
  name: "",
  avatar_url: "",
  html_url: "",
  login: "",
  location: "",
  blog: "",
  company: "",
  twitter_username: "",
  following: "",
  followers: "",
  public_gists: "",
});

interface UserRepo {
  id: number;
  name: string;
  html_url: string;
  description: string;
  language: string;
}
const userRepos = ref<UserRepo[]>([]);
const viewProfile = async () => {
  await axios
    .get(`https://api.github.com/users/${route.params.username}`)
    .then((res) => {
      console.log(res);
      if (res.status == 200) {
        userprofile.value = res.data;
        axios
          .get(res.data.repos_url)
          .then((result) => {
            userRepos.value = result.data;
            console.log("REPOS", result);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    })
    .catch((error) => {
      console.log(error);
    });
};

onMounted(() => {
  viewProfile();
});
</script>

<template>
  <!-- Profile -->
  <div class="container">
    <div class="main-body">
      <div class="row">
        <div class="col-md-3">
          <a href="/">Back to search</a>
        </div>
      </div>
      <div class="row gutters-sm">
        <div class="col-md-4 mb-3">
          <div class="card">
            <div class="card-body">
              <div class="d-flex flex-column align-items-center text-center">
                <img
                  :src="userprofile.avatar_url"
                  alt="Admin"
                  class="rounded-circle"
                  width="150"
                />
                <div class="mt-3">
                  <h4>{{ userprofile.name }}</h4>
                  <a :href="userprofile.html_url" class="text-secondary mb-1">{{
                    userprofile.login
                  }}</a>
                  <p class="text-muted font-size-sm">
                    {{ userprofile.location }}
                  </p>
                </div>
              </div>
            </div>
          </div>
          <div class="card mt-3">
            <ul class="list-group list-group-flush">
              <li
                class="list-group-item d-flex justify-content-between align-items-center flex-wrap"
              >
                <h6 class="mb-0">Website</h6>
                <span class="text-secondary"
                  >https://{{ userprofile.blog }}</span
                >
              </li>
              <li
                class="list-group-item d-flex justify-content-between align-items-center flex-wrap"
              >
                <h6 class="mb-0">Company</h6>
                <span class="text-secondary">{{ userprofile.company }}</span>
              </li>
              <li
                class="list-group-item d-flex justify-content-between align-items-center flex-wrap"
              >
                <h6 class="mb-0">Twitter</h6>
                <span class="text-secondary"
                  >@{{ userprofile.twitter_username }}</span
                >
              </li>
              <li
                class="list-group-item d-flex justify-content-between align-items-center flex-wrap"
              >
                <h6 class="mb-0">Following</h6>
                <span class="text-secondary">{{ userprofile.following }}</span>
              </li>
              <li
                class="list-group-item d-flex justify-content-between align-items-center flex-wrap"
              >
                <h6 class="mb-0">Followers</h6>
                <span class="text-secondary">{{ userprofile.followers }}</span>
              </li>
              <li
                class="list-group-item d-flex justify-content-between align-items-center flex-wrap"
              >
                <h6 class="mb-0">Gits</h6>
                <span class="text-secondary">{{
                  userprofile.public_gists
                }}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="col-md-8">
          <div class="card mb-3">
            <div class="card-body">
              <div class="row">
                <div class="col-sm-9 text-secondary">
                  Public Repositories
                  <span class="badge bg-secondary">{{ userRepos.length }}</span>
                </div>
              </div>
              <hr />
              <div v-for="repos in userRepos" :key="repos.id">
                <a :href="repos.html_url"
                  ><h5 class="card-title">{{ repos.name }}</h5></a
                >
                <p class="card-text">
                  {{ repos.description }}
                </p>
                <p class="card-text">
                  <small class="text-muted">{{ repos.language }} </small>
                </p>
                <hr />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
body {
  margin-top: 20px;
  color: #1a202c;
  text-align: left;
  background-color: #e2e8f0;
}
.main-body {
  padding: 15px;
}
.card {
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
}

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 0 solid rgba(0, 0, 0, 0.125);
  border-radius: 0.25rem;
}

.card-body {
  flex: 1 1 auto;
  min-height: 1px;
  padding: 1rem;
}

.gutters-sm {
  margin-right: -8px;
  margin-left: -8px;
}

.gutters-sm > .col,
.gutters-sm > [class*="col-"] {
  padding-right: 8px;
  padding-left: 8px;
}
.mb-3,
.my-3 {
  margin-bottom: 1rem !important;
}

.bg-gray-300 {
  background-color: #e2e8f0;
}
.h-100 {
  height: 100% !important;
}
.shadow-none {
  box-shadow: none !important;
}
</style>
