<script setup>
import { defineExpose, defineProps } from 'vue';
defineExpose({ test })

const emit = defineEmits(['onClick'])

const props = defineProps({
    id: {
        type: Number,
        require: true,
    }
})

function test() {
    console.log(`tile - ${props.id} clicked`)
}
</script>

<template>
    <div class="tile" @click="emit('onClick', props.id)"></div>
</template>

<style lang="scss" scoped>
.tile {
    filter: hue-rotate(10rad);

    width: 100%;
    height: 100%;
    background-color: red;
    border-radius: 6px;
    box-shadow: 0px 0px 5px $gray inset;
    opacity: .7;

    transition: box-shadow .05s;

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
}
</style>