<template>
  <form @submit.prevent>
    <div class="mb-3">
      <label>First Name</label>
      <input v-model="form.first_name" type="text" class="form-control" name="first_name">
    </div>
    <div class="mb-3">
      <label>Last Name</label>
      <input v-model="form.last_name" type="text" class="form-control" name="last_name">
    </div>
    <div class="mb-3">
      <label>Email</label>
      <input v-model="form.email" type="text" class="form-control" name="email">
    </div>
    <div class="mb-3">
      <label>Role</label>
      <select v-model="form.role_id" name="role_id"  class="form-control">
        <option v-for="role of roles" :key="role.id" value="role.id">{{role.name}}</option>
      </select>
    </div>

    <button class="btn btn-outline-secondary" @submit="submit">Save</button>
  </form>
</template>

<script>
import {reactive, onMounted, ref} from "vue";
import axios from 'axios';
import {useRouter} from "vue-router";
export default {
  name: "UserCreate",
  setup() {
    const form = reactive({
      first_name: '',
      last_name: '',
      email: '',
      role_id: ''
    });
    const roles = ref([]);
    const router = useRouter();


    const submit = async () => {
      await axios.post('users', form);
      await router.push('/users');

    }

    onMounted(async () => {
      const {data} = await axios.get('roles');
      roles.value = data;
    })

    return {
      form,
      submit
    }
  }
}
</script>

<style>

</style>