<script setup>
    const props = defineProps({
        btnName: String
    })
</script>

<template>
    <button
        type="button"
        class="btn btn-lg btn-info text-white shadow-none"
        @click="$emit('clickEvent')"
    >{{ props.btnName }}
    </button>
</template>

<style scoped>

</style>