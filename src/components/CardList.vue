<script setup>
import Card from './Card.vue'

defineProps({
    items: Array
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
    <div class="grid grid-cols-4 gap-5">
        <Card 
        v-for="item in items.slice(0, 12)"
        :key="item.id"
        :id = "item.id"
        :title="item.title" 
        :image-url="item.imageUrl" 
        :price="item.price"
        :is-added="false"
        :is-favorite="item.isFavorite"
        :on-click-favorite="() => emit('addToFavorite', item)"
        :on-click-add="() => emit('addToCart', item)"
        :isAdded="item.isAdded"
        />
    </div>


</template>