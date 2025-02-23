<script setup>
import ConnectDevice from "./components/ConnectDevice.vue";
import MiddleCard from "./components/MiddleCard.vue";
import { ref, computed, watch } from "vue";

const device = ref(null);
const deviceName = ref(null);
const deviceInfo = ref(null);
const connected = ref(false);
const server = ref(null);

computed(() => {
    if (connected.value) {
        console.log("Connected to device");
    } else {
        console.log("Not connected to device");
    }
});

watch([device, deviceName, deviceInfo, connected], () => {
    console.log("Device:", device.value);
    console.log("Device Name:", deviceName.value);
    console.log("Device Info:", deviceInfo.value);
    console.log("Connected:", connected.value);
});

const findDevice = async () => {
    console.log("Finding device");
    device.value = await navigator.bluetooth
        .requestDevice({
            filters: [{ name: "homeAutomationPico" }],
            optionalServices: ["00001234-0000-1000-8000-00805f9b34fb"],
        })
        .catch((error) => {
            console.error("Argh! ", error);
        });

    deviceName.value = device.value || "No device found";
    deviceInfo.value = "Connected to: " + deviceName.value;
    connected.value = true;
};

watch(connected, () => {
    if (connected.value) {
        server.value = device.value.gatt.connect();
        console.log("Connected to device payload sent");
    } else {
        console.log("Not connected to device");
    }
});

const disconnectDevice = () => {
    console.log("Disconnecting device");
    device.value.gatt.disconnect();
    connected.value = false;
};
</script>

<template>
    <ConnectDevice
        @findDevice="findDevice"
        :connected="connected"
        @disconnectDevice="disconnectDevice"
    />
    <MiddleCard />
</template>

<style scoped></style>
