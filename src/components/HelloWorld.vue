<template>
  <div>
    <div class="repo-data">
      <ul>
        <li v-for="repo in data" :key="repo">{{ repo}} </li>

      </ul>
    </div>

    <input type="text" v-model="userName" placeholder="Enter GitHub username" />
    <button @click="submitInput">Submit</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const userName = ref('');
const data = ref([]);

const submitInput = async () => {
  try {
    const response = await fetch(`https://api.github.com/users/${userName.value}/repos`);
    const json = await response.json();

    if (json.length > 0) {
      data.value = json.map(repo => repo.name); 
    } else {
      data.value = ['No data found for the specified user'];
    }
  } catch (error) {
    data.value = ['An error occurred while fetching data'];
  }
};
</script>

<style lang="css" scoped>

</style>
