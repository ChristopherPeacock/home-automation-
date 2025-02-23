<script setup>
import ConnectDevice from "./components/ConnectDevice.vue";
import MiddleCard from "./components/MiddleCard.vue";
import { ref, watch } from "vue";

const device = ref(null);
const deviceName = ref(null);
const deviceInfo = ref(null);
const connected = ref(false);
const server = ref(null);

watch(connected, () => {
    console.log("Connected:", connected.value);
});

const findDevice = async () => {
    try {
        console.log("Finding device...");
        device.value = await navigator.bluetooth.requestDevice({
            filters: [{ name: "homeAutomationPico" }],
            optionalServices: ["00001234-0000-1000-8000-00805f9b34fb"],
        });

        if (!device.value) {
            console.error("No device found.");
            return;
        }

        console.log("Device found:", device.value.name);
        deviceName.value = device.value.name;
        deviceInfo.value = "Connected to: " + device.value.name;

        server.value = await device.value.gatt.connect();
        connected.value = true;

        // Watch for disconnection
        device.value.addEventListener("gattserverdisconnected", () => {
            console.log("Device disconnected");
            connected.value = false;
        });

        console.log("Connected to device payload sent");
    } catch (error) {
        console.error("Error finding device:", error);
    }
};

const disconnectDevice = () => {
    if (device.value && device.value.gatt.connected) {
        console.log("Disconnecting device...");
        device.value.gatt.disconnect();
    }
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
