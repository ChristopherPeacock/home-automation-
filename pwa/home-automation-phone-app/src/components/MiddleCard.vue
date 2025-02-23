<script setup>
import { ref } from "vue";

const cards = [
    {
        title: "Temperature",
        value: ref(20),
        id: "cardTemperature",
    },
    {
        title: "Humidity",
        value: ref(20),
        id: "cardHumidity",
    },
    {
        title: "Co2",
        value: ref(20),
        id: "cardCo2",
    },
];

const Temp = cards[0].value;
const humidity = cards[1].value;
const C02 = cards[2].value;

const page = ref(2);
const pages = ref(cards[page]);

const disablePreviousButton = ref(false);
const disableNextButton = ref(false);

const previousCard = () => {
    console.log("Previous Card");
    if (page.value > 1) {
        page.value--;
        if (page.value < 3) {
            disableNextButton.value = false;
        }
        if (page.value < 2) {
            disablePreviousButton.value = true;
        }
    }
};

const nextCard = () => {
    console.log("Next Card");
    if (page.value < 3) {
        page.value++;
        if (page.value > 1) {
            disablePreviousButton.value = false;
        }
        if (page.value > 2) {
            disableNextButton.value = true;
        }
    }
};
</script>

<template>
    <div class="card bg-blue-700 w-96 shadow-xl">
        <div
            class="card-body flex flex-col items-center justify-center"
            id="cardTemperature"
            v-if="page === 1"
        >
            <h1 class="text-2xl text-white">Temperature</h1>
            <div class="radial-progress" style="--value: 20" role="progressbar">
                {{ Temp }}°C
            </div>
        </div>
        <div
            class="card-body flex flex-col items-center justify-center"
            id="cardHumidity"
            v-if="page === 2"
        >
            <h1 class="text-2xl text-white">Humidity</h1>
            <div class="radial-progress" style="--value: 20" role="progressbar">
                {{ humidity }} %
            </div>
        </div>
        <div
            class="card-body flex flex-col items-center justify-center"
            id="cardCo2"
            v-if="page === 3"
        >
            <h1 class="text-2xl text-white">Co2</h1>
            <div class="radial-progress" style="--value: 20" role="progressbar">
                {{ C02 }} ppm
            </div>
        </div>
        <div class="card-footer flex justify-between w-full">
            <button
                class="btn btn-primary visibility-hidden"
                :class="{ invisible: disablePreviousButton }"
                @click="previousCard"
            >
                <
            </button>
            <button
                class="btn btn-primary visibility-hidden"
                :class="{ invisible: disableNextButton }"
                @click="nextCard"
            >
                >
            </button>
        </div>
    </div>
</template>
