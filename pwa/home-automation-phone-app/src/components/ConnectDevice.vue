<script setup>
import { defineEmits } from "vue";
const emit = defineEmits(["findDevice"]);

const findDevice = () => {
    emit("findDevice", findDevice);
};
</script>

<template>
    <div
        class="absolute top-3 right-3 w-auto h-auto flex flex-col items-center justify-center"
    >
        <button @click="findDevice" class="btn btn-wide">Connect Device</button>
    </div>
</template>
