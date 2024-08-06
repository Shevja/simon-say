<script setup>
import { onMounted, ref } from 'vue';
import TheBoard from './components/TheBoard.vue';
import TheInterface from './components/TheInterface.vue';

const dificult = ref('easy')

const tilesCount = ref(4)
const delayTile = ref(500)
const delayRound = ref(1000)

function setBoardSetup(chosenDificult) {
    dificult.value = chosenDificult

    switch (chosenDificult) {
        case 'easy':
            tilesCount.value = 3
            delayTile.value = 500
            delayRound.value = 1000
            break;

        case 'normal':
            tilesCount.value = 4
            delayTile.value = 500
            delayRound.value = 500
            break;

        case 'hard':
            tilesCount.value = 6
            delayTile.value = 250
            delayRound.value = 500
            break;

        case 'pro':
            tilesCount.value = 7
            delayTile.value = 100
            delayRound.value = 500
            break;
    }

    console.log(tilesCount.value, delayTile.value, delayRound.value)
}

onMounted(() => {
    setBoardSetup(dificult.value)
})

</script>

<template>
    <TheInterface @changedDificult="setBoardSetup" />
    <TheBoard :tilesCount="tilesCount" :delayTile="delayTile" :delayRound="delayRound" />
</template>

<style scoped></style>
