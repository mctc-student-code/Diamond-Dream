<template>
    <div>
        <h3>{{ product.productName }}</h3>

        <img v-bind:src="product.image">

        <p>{{ product.description }}</p>

        <p v-if="product.available">This product is available!</p>
        <p v-else>*Sorry, this product is not available. Please check back later.</p>

        <p>Quantity Available: {{ product.quantityAvailable }}</p>

        <ul>
            <li v-for="feature in product.features" v-bind:key="feature">{{ feature }}</li>
        </ul>

        <div>
            <div>
                <p>The maximum quantity you can order is {{ product.maxQuantity }}</p>
                <button v-on:click="decreaseQuantity(product)">-</button>
                <!-- v-model quantity instead product.quantity -->
                <input type="number" v-model="quantity" v-on:change="verifyQuantity(product)">
                <button v-on:click="increaseQuantity(product)">+</button>
            </div>
            <!-- Show the message -->
            <p class="error">{{ message }}</p>
        </div>
        
        <button v-on:click="order(product)">Order</button>

    </div> <!-- End of div -->
</template>

<script>
export default {
    name: 'Product',
    // props - data that something else - another component provides
    props: {
        product: Object, // name: Type
        productApiUrl: String // add new prop
    },
    // data that the component generates
    data() {
        return {
            // Vue component data
            // must be a function
            message: '',
            quantity: 0 // two data items
        }
    }, // don't forget the comma
    methods: {
        order(product) {
            if (this.quantity <= 0) {
                this.message = 'Quantity must be more than 0'
            } else {
                this.message = ''
                this.$emit('product-ordered', this.product.id, this. quantity)
            }
        },
        decreaseQuantity(product) { // include product argument
            this.message = '' 
            let newQuantity = this.quantity - 1
            if ( newQuantity < product.minQuantity) {
                this.message = 'Minimum quantity is ' + product.minQuantity
            } else {
                this.quantity = newQuantity
            }
        },
        increaseQuantity(product) {
            this.message = ''
            let newQuantity = this.quantity + 1
            if ( newQuantity > product.maxQuantity) {
                this.message = 'Maximum quantity is ' + product.maxQuantity
            } else {
                this.quantity = newQuantity
            }
        },
        verifyQuantity(product) {
            this.message = ''
            if ( this.quantity < product.minQuantity ) {
                this.messsage = 'Minimum quantity is ' + product.minQuantity
                this.quantity = product.minQuantity
            }
            if ( this.quantity > product.maxQuantity) {
                this.message = 'Maximum quantity ' + product.maxQuantity
                this.quantity = product.maxQuantity
            }
        }
    }
}
</script>

<style scoped>
    img {
        height: 300px;
    }
    .error {
        color: red;
        font-weight: bold;
    }
/* get the bullet points next to the list items */
    li {
         list-style-position: inside;
         position: relative;
         right: 20px;
    }
</style>