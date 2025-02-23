<script setup>
import ConnectDevice from "./components/ConnectDevice.vue";
import MiddleCard from "./components/MiddleCard.vue";
import { ref } from "vue";

const device = ref(null);
const deviceName = ref(null);
const deviceInfo = ref(null);

const connected = ref(false);

const findDevice = async () => {
    console.log("Finding device");
    device.value = navigator.bluetooth
        .requestDevice({
            filters: [{ name: "homeAutomationPico" }],
            optionalServices: ["6E400001-B5A3-F393-E0A9-E50E24DCCA9E"],
        })
        .then((device) => {
            console.log("Got device:", device.name);
        })
        .catch((error) => {
            console.error("Argh! ", error);
        });

    deviceName.value = device.value.name || "No device found";
    deviceInfo.value = "Connected to: " + deviceName.value;
};
</script>

<template>
    <ConnectDevice @findDevice="findDevice" />
    <MiddleCard />
</template>

<style scoped></style>
