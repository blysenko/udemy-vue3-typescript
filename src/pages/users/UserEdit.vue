<template>
  <form @submit.prevent>
    <div class="mb-3">
      <label>First Name</label>
      <input v-model="data.first_name" type="text" class="form-control" name="first_name">
    </div>
    <div class="mb-3">
      <label>Last Name</label>
      <input v-model="data.last_name" type="text" class="form-control" name="last_name">
    </div>
    <div class="mb-3">
      <label>Email</label>
      <input v-model="data.email" type="text" class="form-control" name="email">
    </div>
    <div class="mb-3">
      <label>Role</label>
      <select v-model="data.role_id" name="role_id"  class="form-control">
        <option v-for="role of roles" :key="role.id" value="role.id">{{role.name}}</option>
      </select>
    </div>

    <button class="btn btn-outline-secondary" @submit="submit">Save</button>
  </form>
</template>

<script>
import {reactive, onMounted, ref} from "vue";
import axios from 'axios';
import {useRouter, useRoute} from "vue-router";
export default {
  name: "UserEdit",
  setup() {
    const data = reactive({
      first_name: '',
      last_name: '',
      email: '',
      role_id: ''
    });
    const roles = ref([]);
    const router = useRouter(); // - for navigation page
    const route = useRoute(); // for query params


    const submit = async () => {
      await axios.post(`users/${route.query.params.id}`, data);
      await router.push('/users');

    }

    onMounted(async () => {
      const rolesResponse = await axios.get('roles');
      roles.value = rolesResponse.data;

      const response = await axios.get(`users/${route.query.params.id}`);
      data.firstName = response.data.first_name;
      data.last_name = response.data.last_name;
      data.email = response.data.email;
      data.role_id = response.data.role.id;
    })

    return {
      data,
      submit
    }
  }
}
</script>

<style>

</style>