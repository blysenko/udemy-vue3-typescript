<template>
  <nav class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3 fs-6" href="#">Company name</a>

    <nav class="my-2 my-md-8 mr-md-3">
      <router-link to="/profile" class="nav-link text-white px-3 text-decoration-none" href="#">{{ name }}</router-link>
      <a class="nav-link text-white px-3 text-decoration-none" @click="logout">Sign in</a>
    </nav>
  </nav>
</template>

<script>
import {onMounted, ref, computed, watch} from "vue";
import axios from "axios";
import {useStore} from "vuex";

export default {
  name: "NavList",
  setup: function () {
    const name = ref('');
    const store = useStore();
    const user = computed(() => store.state.User.user)

    watch(user, () => {
      name.value = user.value.first_name + ' ' + user.value.last_name;
    })

    const logout = async () => {
      await axios.post('logout');
    }

    return {
      name,
      logout
    }
  }
}
</script>