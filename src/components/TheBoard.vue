<script setup>
import { ref, onMounted, reactive } from 'vue';
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

const tiles = ref({})

const sequence = ref([])

onMounted(() => {
    sequence.value.push(getRandomNumber(props.tilesCount))
    // showSequence()
    console.log(tiles[0])
})

function tileClicked(id) {
    console.log(id)
    console.log(getRandomNumber(props.tilesCount), 'rand')
    console.log(tiles.value[id - 1].test())
}

function getRandomNumber(max) {
    return Math.floor(Math.random() * max) + 1
}

function showSequence() {
    sequence.value.forEach(tileId => {

    })
}

</script>

<template>
    <div class="board">
        <ul class="board__tiles">
            <li v-for="tileId in props.tilesCount" :key="tileId" class="board-tile">
                <BaseTile ref="tiles" :id="tileId" @onClick="tileClicked" />
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