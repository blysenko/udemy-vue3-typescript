<template>
  <div class="pt-3 pb-2 mb-3 border-bottom">
    <a href="javascript:void(0)" @click="exportCSV"></a>
  </div>

  <div class="table-responsive">
      <table class="table table-striped table-sm">
          <thead>
          <tr>
              <th scope="col">#</th>
              <th scope="col">Name</th>
              <th scope="col">Email</th>
              <th scope="col">Total</th>
              <th scope="col">Action</th>
          </tr>
          </thead>
          <tbody>
          <template v-for="order in orders" :key="order.id">
            <tr>
              <td>{{order.id}}</td>
              <td>{{order.name}}</td>
              <td>{{order.email}}</td>
              <td>{{order.total}}</td>
              <td>
                <div class="btn-group mr-2">
                  <a href="javascript:(void(0))" class="btn btn-sm btm-outline-secondary"
                  @click="select(order.id)"
                  >View</a>
                </div>
              </td>
            </tr>
            <tr>
              <td colspan="5">
                <div class="overflow-hidden" :class="selected === order.id ? 'show' : 'hide'">
                  <table>
                    <thead>
                    <tr>
                      <th scope="col">#</th>
                      <th scope="col">Name</th>
                      <th scope="col">Email</th>
                      <th scope="col">Total</th>
                      <th scope="col">Action</th>
                    </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in order.item" :key="item.id">
                        <td>{{item.id}}</td>
                        <td>{{item.product_title}}</td>
                        <td>{{item.quantity}}</td>
                        <td>{{item.price}}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </td>
            </tr>
          </template>
          </tbody>
      </table>
  </div>

  <Paginator :last-page="lastPage" @page-changed="load($event)"/>
</template>

<script lang="ts">
import {ref, onMounted} from "vue";
import axios from 'axios';
import Paginator from "@/components/Paginator.vue";

export default {
  name: "OrdersList",
  components: {Paginator},
  setup() {
      const orders = ref([]);
      const lastPage = ref(0);
      const selected = ref(0);

      onMounted(load);

    const load = async (page = 1) => {
      const {data} = await axios.get(`orders?page=${page}`);
      orders.value = data.data;
      lastPage.value = data.meta.last_page
    }

    const select = (id: number) => selected.value = selected.value !== id ? id : 0;

    const exportCSV = async () => {
      const {data} = await axios.post('export', {}, {responseType: 'blob'});
      const blob = new Blob([data], {type: 'text/csv'})
      const link = document.createElement('a');
      link.href = window.URL.createObjectURL(data);
      link.download = 'orders.csv';
      link.click();
    }

      return {
        orders,
        lastPage,
        selected,
        load,
        select,
        exportCSV
      }
  }
}
</script>
<style scoped>
.show {
  max-height: 150px;
  transition: max-height 1ms ease-in;
}
.hide {
  max-height: 0;
  transition: max-height 1ms out;
}
</style>
