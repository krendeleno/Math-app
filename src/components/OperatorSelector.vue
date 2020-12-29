<template>
    <div>
        <p>{{currentCite}}</p>
        <div class="main_menu">
            <button class="button_add"
                    @mouseover="$emit('changeHover', true, '+')"
                    @mouseout="$emit('changeHover', false, '+')"
                    @click="$emit('changeOperator', '+')">+</button>
            <button class="button_sub"
                    @mouseover="$emit('changeHover', true, '-')"
                    @mouseout="$emit('changeHover', false, '-')"
                    @click="$emit('changeOperator','-')">-</button>
            <button class="button_div"
                    @mouseover="$emit('changeHover', true, '/')"
                    @mouseout="$emit('changeHover', false, '/')"
                    @click="$emit('changeOperator','/')">/</button>
            <button class="button_mul"
                    @mouseover="$emit('changeHover', true, '*')"
                    @mouseout="$emit('changeHover', false, '*')"
                    @click="$emit('changeOperator','*')">*</button>
            <button class="button_mix"
                    @mouseover="$emit('changeHover', true, '&')"
                    @mouseout="$emit('changeHover', false, '&')"
                    @click="$emit('changeOperator', '&')">Смешанный режим</button>
            <button class="button_stats"
                    @mouseover="$emit('changeHover', true, '?')"
                    @mouseout="$emit('changeHover', false, '?')"
                    @click="$emit('showStats')">Стастистка</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "OperatorSelector",
        mounted: function () {
                this.currentCite = this.cites[Math.floor(Math.random() * this.cites.length)];
        },
        data() {
            return {
                currentCite: null,
                isHoveringAdd: false,
                cites: [
                    "А ведь ты мог бы сейчас смотреть аниме.",
                    "Через 50 примеров вы преисполнитесь в своем познании.",
                    "Надо было поступать на матмех.",
                    "Жить, чтобы ботать и ботать, чтобы жить.",
                    "Спонсировано туториалами с Ютуба и ответами на StackOverflow.",
                    "Да, я тоже устал.",
                    "Отправь этот тест 10 друзьям или не сдашь сессию.",
                    "Я просто хочу зачет по веб-технологиям.",
                    "Начну учиться с нового года.",
                    "Здесь могла быть ваша новогодняя шутка."
                ],
            }
        }
    }
</script>

<style scoped>

    p {
        font-size: 1.3vw;
        font-style: italic;
    }

    .main_menu {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 15px 10px;
    }

    .main_menu button {
        font-family: 'Exo 2', sans-serif;
        font-size: 3vw;
        margin-right: 0;
        width: 10vw;
        height: 10vw;
    }

    .button_add, .button_div {
        justify-self: end;
    }
    .button_sub, .button_mul {
        justify-self: start;
    }

    .button_mix {
        font-size: 1.5vw !important;
        width: 20vw !important;
        height: auto !important;
        grid-column: 1 / 3;
        grid-row: 3;
        justify-self: center;
    }

    .button_stats {
        font-size: 1.5vw !important;
        width: 20vw !important;
        height: auto !important;
        grid-column: 1 / 3;
        grid-row: 4;
        justify-self: center;
    }

</style>