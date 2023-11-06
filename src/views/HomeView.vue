<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();
const username = ref("");
const userprofile = ref([]);

const viewProfile = async () => {
  await axios
    .get(`https://api.github.com/users/${username.value}`)
    .then((res) => {
      console.log(res);
      if (res.status == 200) {
        userprofile.value = res.data;
      }
    })
    .catch((error) => {
      console.log(error);
    });
};
</script>

<template>
  <div class="container mt-5">
    <h2>GitHub Profile Viewer</h2>
    <div class="row mt-4">
      <input
        type="text"
        class="form-control"
        placeholder="Enter a GitHub username"
        v-model="username"
      />
    </div>
    <div class="row mt-4">
      <div class="col-md-2">
        <button
          type="button"
          class="btn btn-success"
          :disabled="username.length == 0"
          @click="router.push(`profile/${username}`)"
        >
          View
        </button>
      </div>
    </div>
  </div>
</template>
