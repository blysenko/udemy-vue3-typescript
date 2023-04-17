<template>
  <div class="pt-3 pb-2 mb-3 border-bottom">
    <router-link to="/roles/create" class="btn btn-sm btn-outline-secondary">Add</router-link>
  </div>
  <div class="table-responsive">
    <table class="table table-striped table-sm">
      <thead>
      <tr>
        <th scope="col">#</th>
        <th scope="col">name</th>
        <th scope="col">action</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="role in roles" :key="role.id">
        <td>{{role.id}}</td>
        <td>{{role.name}}</td>
        <td>
          <div class="btn-group mr-2">
            <router-link :to="`/users/${user.id}/edit`" class="btn btn-sm btm-outline-secondary">Edit</router-link>
            <a href="javascript:(void(0))" class="btn btn-sm btm-outline-secondary" @click="del(role.id)">Delete</a>
          </div>
        </td>
      </tr>

      </tbody>
    </table>
  </div>

  <nav>
    <ul class="pagination">
      <li class="page-item">
        <a class="page-link" href="javascript:(void(0))" @click="prev">Previous</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="javascript:(void(0))" @click="next">Next</a>
      </li>
    </ul>
  </nav>
</template>

<script lang="ts">

import {onMounted, ref} from "vue";
import axios from 'axios';
import {Role} from "@/models/role";

export default {
  name: "UsersList",
  setup() {
    const roles = ref([]);

    const del = async (id: number) => {
      if(confirm("Are you sure?")) {
        await axios.delete(`roles/${id}`);

        roles.value = roles.value.filter((r: Role) => r.id !== id); // set type of r: to Role
      }
    }

    onMounted(async () => {
      const {data} = await axios.get('roles');

      roles.value = data;
    })
  }
}
</script>

<style scoped>

</style>