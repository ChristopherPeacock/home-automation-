<script setup>
import { defineEmits, ref, defineProps, watch } from "vue";

const props = defineProps({
    connected: Boolean,
});

const emit = defineEmits(["findDevice", "disconnectDevice"]);

const findDevice = () => {
    emit("findDevice");
};

const disconnectDevice = () => {
    emit("disconnectDevice");
};
</script>

<template>
    <div
        class="absolute top-3 right-3 w-auto h-auto flex flex-col items-center justify-center"
    >
        <button
            v-if="!props.connected"
            @click="findDevice"
            class="btn btn-wide"
        >
            Connect Device
        </button>
        <button
            v-if="props.connected"
            @click="disconnectDevice"
            class="btn btn-wide"
        >
            Disconnect Device
        </button>
    </div>
</template>
