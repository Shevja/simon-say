<script setup>
import { ref, onMounted, reactive, watch } from 'vue';
import BaseTile from './BaseTile.vue';

const props = defineProps({
    tilesCount: {
        type: Number,
        required: true,
    },
    delayTile: {
        type: Number,
        required: true,
    },
    delayRound: {
        type: Number,
        required: true,
    }
})

const board = ref(null)
const boardTileList = ref(null)
const tiles = ref({})

const sequence = ref([])
const step = ref(0)

const isGameStarted = ref(false)

function addNewTileToSequence() {
    const randomTileId = Math.floor(Math.random() * props.tilesCount)
    sequence.value.push(randomTileId)
}

function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms))
}

async function nextRound() {
    step.value = 0
    addNewTileToSequence()

    await sleep(props.delayRound)
    showSequence()
}

function tileClicked(id) {
    tiles.value[id].playAudio(true)
    const currentId = sequence.value[step.value]

    if (id === currentId) {
        console.log('success')
        step.value++
    } else {
        gameOver()
        return
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

        await sleep(props.delayTile)
    }

    board.value.style.pointerEvents = ''
}

function resetBoard() {
    sequence.value = []
    step.value = 0
}

function gameOver() {
    resetBoard()

    console.log('game over')
}

function startGame() {
    resetBoard()

    addNewTileToSequence()
    showSequence()
}

onMounted(() => {
    // addNewTileToSequence()
    // showSequence()

    window.addEventListener('resize', () => {
        const tileWidth = boardTileList.value.children[0].offsetWidth

        if (!tilesCount % 2) {
            // const lastTile = boardTileList.value.children[props.tilesCount - 1]
            // console.log(lastTile)
        }
    })
})

watch(
    () => props.tilesCount,
    (tilesCount) => {
        resetBoard()

        const tileList = boardTileList.value
        const tileElems = [...tileList.children]
        const tileWidth = tileElems[0].offsetWidth
        
        tileList.style.maxWidth = `${Math.round(tilesCount / 2) * tileWidth}px`
    }
)

</script>

<template>
    <section class="board">
        <div class="container">
            <button class="button" @click="startGame">
                Начать игру
            </button>
            <div ref="board" class="board__wrapper">
                <ul ref="boardTileList" class="board__tiles">
                    <li v-for="tileId in props.tilesCount" :key="tileId" class="board-tile">
                        <BaseTile ref="tiles" :id="tileId - 1" @onMousedown="tileClicked" />
                    </li>
                </ul>
            </div>
        </div>
    </section>
</template>

<style lang="scss">
.board {
    padding: 20px 0 40px;

    &__wrapper {
        display: flex;
        justify-content: center;
    }

    &__tiles {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        width: 100%;
    }

    &-tile {
        flex: 1 0 calc(50% - 5px);
        max-width: 200px;
        aspect-ratio: 1 / 1;
    }
}
</style>