<template>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">name</th>
        <th scope="col">email</th>
        <th scope="col">role</th>
        <th scope="col">action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="user in users" :key="user.id">
        <td>{{ user.id }}</td>
        <td>{{ user.first_name }} {{ user.last_name }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.role.name }}</td>
        <td></td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">

import {onMounted, ref} from "vue";
import axios from 'axios';

export default {
name: "UsersList",
  setup() {
    const users = ref([]);

    onMounted(async () => {
      const {data} = await axios.get('users');
      users.value = data.data;
    })

    return {
      users
    }
  }
}
</script>

<style scoped>

</style>