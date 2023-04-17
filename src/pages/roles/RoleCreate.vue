<template>
  <form @submit.prevent="submit" class="mb-3 mt-3 row">
    <div>
      <label class="col-sm-2 col-form-label">Name</label>
      <div class="col-sm-10">
        <input v-model="formData.name" class="form-control" name="name">
      </div>
    </div>

    <div class="mb-e row">
      <label class="col-sm-2col-form-label">Premission</label>
      <div class="col-sm-10">
        <div class="form-check form-check-inline col-3" v-for="permission in formData.permissions" :key="permission.id">
          <input type="checkbox" class="form-check-input" :value="permission.id" @change="select(permission.id, $event.target.checked)">
          <label class="form-check-label">{{permission.name}}</label>
        </div>
      </div>
    </div>
    <button class="btn btn-outline-secondary">Save</button>
  </form>
</template>

<script lang="ts">
import {onMounted, reactive, ref} from 'vue';
import {useRouter} from 'vue-router';
import axios from 'axios';

export default {
  name: "RoleCreate",
  setup() {
    const {push} = useRouter();
    const formData = reactive({
      name: '',
      permission: [] as number[]
    })
    const permissionList = ref([]);

    const select = async (id: number, checked: boolean) => {
      if (checked) {
        formData.permission = [...formData.permission, id];
        return;
      }
      formData.permission = formData.permission.filter(p => p !== id);
    }

    const submit = async () => {
      await axios.post('roles', formData);
      await push('/roles');
    }



    onMounted(async () => {
      const {data} = await axios.get(`permissions/params.id`);
      permissionList.value = data;
    })

    return {
      formData,
      permissionList,
      select,
      submit
    }
  }
}
</script>