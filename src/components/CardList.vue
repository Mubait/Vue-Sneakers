<script setup>
import Card from './Card.vue'

defineProps({
    items: Array,
    isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
    <div class="grid grid-cols-4 gap-5" v-auto-animate>
        <Card 
        v-for="item in items.slice(0, 12)"
        :key="item.id"
        :id = "item.id"
        :title="item.title" 
        :image-url="item.imageUrl" 
        :price="item.price"
        :is-added="false"
        :is-favorite="item.isFavorite"
        :on-click-favorite="isFavorites ? null : () => emit('addToFavorite', item)"
        :on-click-add="isFavorites ? null : () => emit('addToCart', item)"
        :isAdded="item.isAdded"
        />
    </div>


</template>