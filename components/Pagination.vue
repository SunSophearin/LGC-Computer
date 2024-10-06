<script setup lang="ts">
const props = defineProps({
    page: {
        type: Number,
        default: 1
    },
    items: {
        type: Array
    }
})
</script>
<template>
    <UPagination v-model="props.page" :page-count="5" :total="props.items.length" />
</template>
  