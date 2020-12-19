<template>
    <div id='Home'>
        <Header></Header>
        <img alt="Vue logo" class="logo" src="../assets/logo.png">

        <h1 v-if="hasError">API no disponible.</h1>

        <div v-else>
            <h1 v-if="loading">Cargando productos...</h1>
            <div v-else>
                <h1>Productos</h1>
                <div class="newProduct">
                    Nombre:<input v-model="name" type="text">
                    Precio:<input v-model="price" type="text">
                    <button class="addProduct-btn" @click="addProduct( name, price )">Agregar</button>
                    <button class="addProduct-btn" @click="updateProduct( name, price )">Update</button>
                </div>

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
        addProduct(nameProduct, priceProduct) {
            const newProduct = {
                name: nameProduct,
                price: priceProduct
            };
            axios.post('http://127.0.0.1:8000/api/products/', newProduct)
            .then(response => {this.products.push(response.data.data);
            alert("Producto Añadido")})
        },
        // eslint-disable-next-line no-unused-vars
        updateProduct(nameProduct, priceProduct) {
            const newProduct = {
                id: 1,
                name: "nameProduct",
                price: 12314
            };
            axios.put('http://127.0.0.1:8000/products/' + newProduct.id, newProduct)
            .then(response => {this.products.push(response.data.data);
            alert("Producto Actualizado")})
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