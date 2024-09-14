<script setup>
  import axios from 'axios'
  import { ref, watch, provide, computed } from 'vue'

  import Header from './components/Header.vue'
  import Drawer from './components/Drawer.vue'
  import Home from './pages/Home.vue';
import Favorites from './pages/Favorites.vue';


  /* Корзина {START}*/
  const cart = ref([])
  const isCreatingOrder = ref(false)

  const drawerOpen = ref(false)

  const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

  const cartIsEmpty = computed(() => cart.value.length === 0)
  const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value);

  const closerDrawer = () => {
    drawerOpen.value = false;
  }
  const openDrawer = () => {
    drawerOpen.value = true;
  }

  const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
  }

  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false;
  }

  const createOrder = async() => {
    try {
      isCreatingOrder.value = true
      const { data } = await axios.post('https://5b70284b34591f86.mokky.dev/orders', {
        items: cart.value,
        totalPrice: totalPrice.value,
      })

      cart.value = []

      return data;
    } catch (err) {
      console.log(err)
    } finally {
      isCreatingOrder.value = false
    }
  }

  watch(
    cart, 
    () => {
      localStorage.setItem('cart', JSON.stringify(cart.value))
    }, 
    { deep: true}
  )

  provide('cart', {
    cart,
    closerDrawer,
    openDrawer,
    addToCart,
    removeFromCart
  });
  /* Корзина {END} */
  
</script>

<template>
  <Drawer v-if="drawerOpen" 
  :total-price="totalPrice" 
  :vatPrice="vatPrice" 
  @create-order="createOrder"
  :button-disabled="cartButtonDisabled"
  />


  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer"/>
    <div class="p-10">
      <RouterView />
    </div>
  </div>

</template>