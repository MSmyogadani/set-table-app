<script setup>
    import { ref, watch } from 'vue'

    const emit = defineEmits(['inputEvent'])
    const props = defineProps({
        tableNumber: Object
    })
    const seatCount = ref(props.tableNumber['seatCount'])

    function inputEvent(value) {
        if (value < 0) {
            seatCount.value = 0
        }
        emit('inputEvent', { number: props.tableNumber['number'], seatCount: value < 0 ? 0 : value })
    }
</script>

<template>
    <div class="table_number input-group input-group-ig">
        <span class="input-group-text fs-3 fw-bold">{{ props.tableNumber['number'] }}</span>
        <input
            type="number"
            class="fs-1 text-center form-control form-control-lg shadow-none"
            :value="props.tableNumber['seatCount']"
            @input="inputEvent($event.target.value)"
        >
    </div>
</template>

<style scoped>
    .table_number {
        height: 80px;
    }
</style>