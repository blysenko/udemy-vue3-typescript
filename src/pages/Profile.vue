<template>
  <h3>Account information</h3>
  <form @submit.prevent="infoSubmit" action="">
    <div class="mb-3">
      <label for="">First Name</label>
      <input v-model="infoData.first_name" name="first_name" class="form-control">
    </div>
    <div class="mb-3">
      <label for="">Last Name</label>
      <input v-model="infoData.last_name" name="last_name" class="form-control">
    </div>
    <div class="mb-3">
      <label for="">Email</label>
      <input v-model="infoData.email" name="email" class="form-control">
    </div>

    <button class="btn btn-outline-secondary">Save</button>
  </form>

  <h3 class="mt-4">Change password</h3>
  <form @submit.prevent="passwordSubmit">
    <div class="mb-3">
      <label for="">Password</label>
      <input v-model="passwordData.password" type="password" name="password" class="form-control">
    </div>
    <div class="mb-3">
      <label for="">Password Confirm</label>
      <input v-model="passwordData.password_confirm" type="password" name="password_confirm" class="form-control">
    </div>

    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script setup lang="ts">
import {reactive, onMounted} from 'vue';
import axios from "axios";
export default {
  name: 'ProfilePage',
  setup() {
    const infoData = reactive({
      first_name: '',
      last_name: '',
      email: ''
    });

    const passwordData = reactive({
      password: '',
      password_confirm: ''
    });

    onMounted(async () => {
      const {data} = await axios.get('user');
      infoData.first_name = data.first_name
      infoData.last_name = data.last_name
      infoData.email = data.email
    })

    const infoSubmit = async () => {
      await axios.put('users/info', infoData);
    }
    const passwordSubmit = async () => {
      await axios.put('users/passwword', passwordData);
    }

    return {
      infoData,
      passwordData,
      infoSubmit,
      passwordSubmit
    }
  }
}
</script>