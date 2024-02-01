<script setup>
  import {ref, onMounted, watch } from 'vue'
  import {db} from './data/data';
  import Guitar from './components/guitar/Guitar.vue'
  import Header from './components/UI/header/Header.vue'
  import Footer from './components/UI/footer/Footer.vue'

  const guitars = ref([])
  const cart = ref([])
  const guitar = ref({})

  watch(cart, () => {
    watchLocalStorage()
  }, {
    deep: true
  })

  onMounted(() => {
    guitars.value = db
    guitar.value = db[3]

    const cartStorage = localStorage.getItem('cart')
    if(cartStorage) {
      cart.value = JSON.parse(cartStorage)
    }
  })

  const watchLocalStorage = () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  }

  const addCart = (guitar) => {
      const existCart = cart.value.findIndex(product => product.id === guitar.id)
      if(existCart >= 0) {
        cart.value[existCart].quantity++
      } else {
        guitar.quantity = 1;
        cart.value.push(guitar);
      }
  }

  const decrementQuantity = (id) => {
    const index = cart.value.findIndex(product => product.id === id)
    if(cart.value[index].quantity <= 1) return
    cart.value[index].quantity--
  }

  const incrementQuantity = (id) => {
    const index = cart.value.findIndex(product => product.id === id)
    if(cart.value[index].quantity >= 5) return
    cart.value[index].quantity++
  }

  const removeProoduct = (id) => {
    cart.value = cart.value.filter(product => product.id !== id)
  }

  const emptyCart = () => {
    cart.value = []
  }

</script>

<template>
  <Header 
      :cart="cart"
      :guitar="guitar"
      @decrement-qt="decrementQuantity"
      @increment-qt="incrementQuantity"
      @add-cart="addCart"
      @delete-product="removeProoduct"
      @empty-cart="emptyCart"
  />
    <main class="container-xl mt-5">
        <h2 class="text-center">Our Collection</h2>
        
        <div class="row mt-5">
            <Guitar 
    v-for="guitar in guitars"
    :key="guitar.id"
    :guitar="guitar"
    @add-cart="addCart"
/>

            />
        </div>
    </main>
  <Footer />
</template>

