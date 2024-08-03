<script setup>
import { ref, onMounted, reactive, watch } from 'vue';
import BaseTile from './BaseTile.vue';

const props = defineProps({
    tilesCount: {
        default: 4,
        type: Number,
    },
    dificult: {
        default: 0,
        type: Number,
    },
})

const board = ref(null)
const tiles = ref({})

const sequence = ref([])
const step = ref(0)
const delay = ref(500)

const isGameStarted = ref(false)
const isGameOver = ref(false)

function addNewTileToSequence() {
    sequence.value.push(getRandomNumber(props.tilesCount))
}

function getRandomNumber(max) {
    return Math.floor(Math.random() * max)
}

function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms))
}

function nextRound() {
    step.value = 0
    addNewTileToSequence()
    showSequence()
}

function tileClicked(id) {
    tiles.value[id].playAudio(true)
    const currentId = sequence.value[step.value]

    if (id === currentId) {
        console.log('success')
        step.value++
    } else {
        isGameOver.value = true
    }

    if (step.value >= sequence.value.length) {
        nextRound()
        console.log('next round')
    }
}

async function showSequence() {
    board.value.style.pointerEvents = 'none'

    for (let idx = 0; idx < sequence.value.length; idx += 1) {

        let tileId = sequence.value[idx]
        tiles.value[tileId].playAudio(false)

        await sleep(delay.value)
    }

    board.value.style.pointerEvents = ''
}

onMounted(() => {
    addNewTileToSequence()
    // showSequence()
})

watch(
    () => isGameStarted.value,
    () => {
        isGameOver.value = false
        // delay.value = 0

        showSequence()
    }
)

</script>

<template>
    <button @click="isGameStarted = true">
        start
    </button>
    <div ref="board" class="board">
        <ul class="board__tiles">
            <li v-for="tileId in props.tilesCount" :key="tileId" class="board-tile">
                <BaseTile ref="tiles" :id="tileId - 1" @onMousedown="tileClicked" />
            </li>
        </ul>
    </div>
</template>

<style lang="scss">
.board {
    display: flex;
    justify-content: center;

    &__tiles {
        display: flex;
        flex-wrap: wrap;
        gap: 5px;
        width: 100%;
        max-width: 400px;
    }

    &-tile {
        flex: 1 0 calc(50% - 5px);
        // height: 40px;
        aspect-ratio: 1 / 1;
    }
}
</style>