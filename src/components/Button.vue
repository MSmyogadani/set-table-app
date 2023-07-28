<script setup>
    const props = defineProps({
        btnName: String
    })
</script>

<template>
    <button
        type="button"
        class="btn btn-sm p-2 btn-white border-info text-info shadow-none"
        @click="$emit('clickEvent')"
    >{{ props.btnName }}<slot></slot>
    </button>
</template>

<style scoped>

</style>