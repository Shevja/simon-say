<script setup>
import { onMounted, ref } from 'vue';
defineExpose({ playAudio })

const emit = defineEmits(['onMousedown'])

const props = defineProps({
    id: {
        type: Number,
        require: true,
    }
})

const tile = ref(null)
const tileAudio = ref(null)

function playAudio(isUserClicked) {
    // console.log(`tile - ${props.id} clicked`)

    if (!isUserClicked) {
        tile.value.classList.add('tile_active')
        
        setTimeout(() => {
            tile.value.classList.remove('tile_active')
        }, 200)
    }

    if (tileAudio.value.paused) {
        // здесь возвращается промис который выдает ошибку совместно с showsqs
        tileAudio.value.play()
    } else {
        tileAudio.value.pause()
        tileAudio.value.currentTime = 0
        tileAudio.value.play()
    }
}

function createAudio(tileId) {
    const audio = new Audio()
    audio.preload = 'auto'
    audio.src = `/src/assets/sounds/${tileId}.mp3`
    audio.volume = .5
    audio.playbackRate = 1

    return audio
}

onMounted(() => {
    tileAudio.value = createAudio(props.id)
    tile.value.classList.add(`tile_${props.id}`)
    // console.log(tile.value, props.id)
})

</script>

<template>
    <div ref="tile" class="tile" @mousedown="emit('onMousedown', props.id)"></div>
</template>

<style lang="scss" scoped>
.tile {
    width: 100%;
    height: 100%;
    box-shadow: 0px 0px 5px $gray inset;
    opacity: .7;

    transition: box-shadow .05s, opacity .2s;

    &:hover {
        cursor: pointer;
        opacity: 1;
    }

    &:active {
        box-shadow: 0px 0px 15px $gray inset;
        opacity: .95;
    }

    &_active {
        box-shadow: 0px 0px 15px $gray inset;
        opacity: .95;
    }

    &_0 {
        background-color: $tileWhite;
    }

    &_1 {
        background-color: $tileRed;
    }

    &_2 {
        background-color: $tileGreen;
    }

    &_3 {
        background-color: $tileYellow;
    }

    &_4 {
        background-color: $tileTurquoise;
    }

    &_5 {
        background-color: $tileBlue;
    }

    &_6 {
        background-color: $tileViolet;
    }
}
</style>