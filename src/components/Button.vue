<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    onClick: Function,
})

</script>

<template>
    <button @click="onClick" class="btn">
        <slot></slot>
    </button>
</template>

<style scoped>
.btn {
    background-color: #1e78a6;
    min-width: 300px;
    color: white;
    padding: 20px;
    border-radius: 10px;
}
</style>

