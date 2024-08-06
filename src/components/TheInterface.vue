<script setup>
import { ref, watch } from 'vue'

const emits = defineEmits(['changedDificult'])

const dificult = ref('easy')

watch(
    () => dificult.value,
    () => {
        emits('changedDificult', dificult.value)
    }
)

</script>

<template>
    <section class="interface">
        <div class="container">

            <div class="interface__wrapper">
                <div class="input__wrapper interface-input__wrapper">
                    <label for="username">
                        Ваше имя или никнейм (нужно только для таблицы рекордов)
                    </label>
                    <input class="input" placeholder="Имя или никнейм" type="text" name="username" id="username">
                </div>

                <div class="interface-input__wrapper">
                    <span>
                        Уровень сложности (влияет на количество секторов и скорость игры)
                    </span>
                    <ul class="interface__radios">
                        <li>
                            <input class="input" type="radio" name="dificult" id="easy" value="easy" checked
                                v-model="dificult">
                            <label for="easy">Легкий</label>
                        </li>
                        <li>
                            <input class="input" type="radio" name="dificult" id="normal" value="normal"
                                v-model="dificult">
                            <label for="normal">Средний</label>
                        </li>
                        <li>
                            <input class="input" type="radio" name="dificult" id="hard" value="hard" v-model="dificult">
                            <label for="hard">Сложный</label>
                        </li>
                        <li>
                            <input class="input" type="radio" name="dificult" id="pro" value="pro" v-model="dificult">
                            <label for="pro">Я профессионал!</label>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </section>
</template>

<style lang='scss'>
.interface {
    padding: 40px 0 20px;
    font-size: 18px;

    &__wrapper {
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    &__radios {
        display: flex;
        flex-direction: column;
        gap: 7px;

        &>li {
            display: flex;
            gap: 10px;
            align-items: center;
        }
    }

    &-input {
        &__wrapper {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }
    }
}
</style>