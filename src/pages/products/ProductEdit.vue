<template>
    <form action="">
        <div class="mb-3">
            <label>Title</label>
            <input v-model="data.title" class="form-control" name="title">
        </div>
        <div class="mb-3">
            <label>Description</label>
            <input v-model="data.description" class="form-control" name="description">
        </div>
        <div class="mb-3">
            <label>Image</label>
            <div class="input-group">
                <input v-model="data.image" class="form-control" name="image">
                <ImageUpload @uploaded="data.image = $event"/>
            </div>
        </div>
        <div class="mb-3">
            <label>Price</label>
            <input v-model="data.price" class="form-control" name="price">
        </div>
        <button class="btn btn-outline-secondary">Save</button>
    </form>

</template>

<script lang="ts">
import {reactive, onMounted} from 'vue';
import axios from 'axios';
import {useRouter, useRoute} from 'vue-router';
import ImageUpload from "@/components/ImageUpload.vue";

export default {
    name: "ProductEdit",
    components: {ImageUpload},
    setup() {
        const router = useRouter();
        const {params} = useRoute();

        const data = reactive({
            title: '',
            description: '',
            image: '',
            price: ''
        })
        onMounted(async () => {
            const {data} = await axios.get(`/product&id=${params.id}`);
            data.title = data.data.title;
            data.description = data.data.description;
            data.image = data.data.image;
            data.price = data.data.price;
        })


        const submit = async () => {
            await axios.post('products', data);

            router.push('/products');
        }

        return {
            data,
            submit
        }
    }
}
</script>
