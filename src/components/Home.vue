<template>
    <div id='Home'>
        <Header></Header>
        <img alt="Vue logo" class="logo" src="../assets/logo.png">

        <h1 v-if="hasError">API no disponible.</h1>

        <div v-else>
            <h1 v-if="loading">Cargando productos...</h1>
            <div v-else>
                <h1>Productos</h1>

                <button class="addProduct-btn" @click="addProduct('vueJS')">Agregar</button>

                <table class="products-table" v-for="product in products" :key="product.id">
                    <tr>
                        <th>Nombre</th>
                        <th>Precio</th>
                        <th>Acciones</th>
                    </tr>
                    <tr>
                        <td>
                            {{ product.name }}
                        </td>
                        <td>
                            {{ product.price }}
                        </td>
                        <td>
                            <button @click="removeProduct(product.id)">Eliminar</button>
                        </td>
                    </tr>
                </table>
            </div>
        </div>

    </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import Header from './Header.vue';
import axios from 'axios';

export default {
    name: "Home",
    components: {
        Header
    },
    data() {
        return {
            products: null,
            hasError: false,
            loading: true
        }
    },
    methods: {
        removeProduct(id) {
            axios.delete('http://127.0.0.1:8000/api/products/' + id)
            .then(() => {this.products = this.products.filter(product => product.id !== id);});
        },
        addProduct(nameProduct) {
            const newProduct = {
                name: nameProduct,
                price: 500
            };
            axios.post('http://127.0.0.1:8000/api/products/', newProduct)
            .then(response => {this.products.push(response.data.data)})
        }
    },
    mounted() {
        axios
        .get('http://127.0.0.1:8000/products')
        .then(response => (this.products = response.data.data))
        // eslint-disable-next-line no-unused-vars
        .catch(error => this.hasError = true)
        .finally(() => this.loading = false)
    }
}
</script>