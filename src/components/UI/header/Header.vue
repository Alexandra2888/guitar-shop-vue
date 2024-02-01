<script setup>
    import { computed } from 'vue'

    const props = defineProps({
        cart: {
            type: Array,
            required: true
        },
        guitar: {
            type: Object,
            required: true
        }
    })

    defineEmits(['decrement-quantity', 'increment-quantity', 'add-cart', 'remove-product', 'empty-cart'])

    const totalPrice = computed(() => {
        return props.cart.reduce((total, product) => total + (product.quantity * product.price), 0 )
    })
</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="./../../../../public/img/logo.svg" alt="img logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div 
                        class="cart"
                    >
                        <img class="img-fluid" src="../../../../public/img/cart.png" alt="img cart" />
    
                        <div id="cart" class="bg-white p-3">
                            <p v-if="cart.length === 0" class="text-center m-0">
                                Cart is empty
                            </p>
                            <div v-else>
                                <table   class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th>Name</th>
                                            <th>Price</th>
                                            <th>Quantity</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr
                                            v-for="product in cart"
                                            :key="product.id"
                                        >
                                            <td>
                                                <img 
                                                    class="img-fluid" 
                                                    :src="'/img/' + product.img + '.jpg'" 
                                                    :alt="'img guitar ' + product.name"
                                                >
                                            </td>
                                            <td>
                                                {{ product.name }}
                                            </td>
                                            <td class="fw-bold">
                                                ${{product.price}}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button
                                                    type="button"
                                                    class="btn btn-dark"
                                                    @click="$emit('decrement-qt', product.id)"
                                                >
                                                    -
                                                </button>
                                                    {{ product.quantity }}
                                                <button
                                                    type="button"
                                                    class="btn btn-dark"
                                                    @click="$emit('increment-qt', product.id)"
                                                >
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button
                                                    class="btn btn-danger"
                                                    type="button"
                                                    @click="$emit('delete-product', product.id)"
                                                >
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
        
                                <p class="text-end">Total price: <span class="fw-bold">${{ totalPrice }}</span></p>

                                <button 
                                    class="btn btn-dark w-100 mt-3 p-2"
                                    @click="$emit('empty-cart')"
                                >Empty cart</button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->
    
            <div  class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Model {{guitar.name}}</h1>
                    <p class="mt-5 fs-5 text-white">{{ guitar.description }} </p>
                    <p class="text-primary fs-1 fw-black">${{guitar.price }}</p>
                    <button 
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        @click="$emit('add-cart', guitar)"
                    >Add in Cart</button>
                </div>
            </div>
        </div>
    
        <img class="header-guitar" src="../../../assets/img/header_guitar.png" alt="img header">
    </header>
</template>
