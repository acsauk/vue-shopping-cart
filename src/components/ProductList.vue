<template>
    <div>
        <h1>Product List</h1>
        <img
            v-if="loading"
            src="http://i.imgur.com/JfPpwOA.gif"
        >
        <ul v-else>
            <li v-for="product in products">
                {{ product.title }} - {{ product.price | currency }} - {{ product.inventory }}
                <button
                    :disabled="!productIsInStock(product)"
                    @click="addProductToCart(product)"
                >Add to cart</button>
            </li>
        </ul>
    </div>
</template>

<script>
    import {mapState, mapGetters, mapActions} from 'vuex'

    export default {
        data () {
            return {
                loading: false
            }
        },

        // 6 minute mark of Vuex modules video - should change to products.products.items but not working
        computed: {
            ...mapState({
                products: state => state.products.products
            }),

            ...mapGetters({
                productIsInStock: 'productIsInStock'
            }),
        },

        methods: {
            ...mapActions({
                fetchProducts: 'fetchProducts',
                addProductToCart: 'addProductToCart'
            })
        },

        created() {
            this.loading = true
            this.fetchProducts()
                .then(() => this.loading = false)
        }
    }
</script>

<style scoped>

</style>