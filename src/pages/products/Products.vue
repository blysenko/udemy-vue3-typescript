<template>
    <div class="table-responsive">
        <table class="table table-striped table-sm">
            <thead>
            <tr>
                <th scope="col">#</th>
                <th scope="col">Image</th>
                <th scope="col">Title</th>
                <th scope="col">Description</th>
                <th scope="col">Price</th>
                <th scope="col">Action</th>
            </tr>
            </thead>
            <tbody>
                <tr v-for="product in products" :key="product.id">
                    <td>{{ product.id }}</td>
                    <td><img :src="product.image" width="50"></td>
                    <td>{{ product.title }}</td>
                    <td>{{ product.description }}</td>
                    <td>{{ product.price }}</td>
                    <td>
                        <div class="btn-group mr-2">
                            <router-link :to="`/products/${product.id}/edit`" class="btn btn-sm btm-outline-secondary">Edit</router-link>
                            <a href="javascript:(void(0))" class="btn btn-sm btm-outline-secondary" @click="del(product.id)">Delete</a>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>

    <Paginator :last-page="lastPage" @page-changed="load($event)"/>
</template>

<script lang="ts">
import {onMounted, ref} from 'vue';
import axios from 'axios';
import {Product} from "@/models/product";
import Paginator from "@/components/Paginator.vue";

export default {
    name: "ProductsList",
    setup() {
        const products = ref([])
        const lastPage = ref(0)

        const load = async(page = 1) => {
            const {data} = await axios.get(`/products?page=${page}`)
            products.value = data.data
            lastPage.value = data.meta.last_page
        }

        onMounted(load)

        const del = async (id: number) => {
            if(confirm("Are you sure?")) {
                await axios.delete(`products/${id}`); // delete from back-side

                products.value = products.value.filter((p: Product) => p.id !== id); // set type of p: to Products - delete from front-side
            }
        }

        return {
            products,
            lastPage,
            del,
            load
        }
    }

}
</script>

<style scoped>

</style>