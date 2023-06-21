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
import {reactive, computed, watch} from 'vue';
import axios from "axios";
import {useStore} from "vuex";
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

    const store = useStore();
    const user = computed(() => store.state.User.user)

    watch(user, () => {
      infoData.first_name = user.value.first_name
      infoData.last_name = user.value.last_name
      infoData.email = user.value.email
    })

    const infoSubmit = async () => {
      const {data} = await axios.put('users/info', infoData);
      await store.dispatch('User/setUser', data);
    }
    const passwordSubmit = async () => {
      await axios.put('users/password', passwordData);
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