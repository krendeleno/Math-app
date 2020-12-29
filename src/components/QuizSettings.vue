<template>
    <div class="settings">
        <h4>Выберите количество вопросов</h4>
        <button @click="newQuiz(5)"> 5 </button>
        <button @click="newQuiz(10)"> 10 </button>
        <button @click="newQuiz(20)"> 20 </button>
        <button class="userButton" @click="numberInput()"> Ввести свое количество вопросов </button>
        <button class="infinityMode" @click="newQuiz(Infinity)"> Бесконечный режим </button>
        <button class="back" @click="$emit('onBack')">Назад</button>
        <div class="user" v-if="userInput">

            <form @submit.prevent="newQuiz(number)">
                <input v-model="number" type="text"/>
                <button class="submitNumber" type="submit">Начать</button>
            </form>
        </div>
        <br>
        <div class="checkboxes">
            <input type="checkbox" id="time" v-model="isTime" @change="$emit('changeTime', isTime)">
            <label for="time">Скоростной режим</label>
            <input type="checkbox" id="hard" v-model="isHard" @change="$emit('changeHard', isHard)">
            <label for="hard">Режим хардкора</label>
        </div>
    </div>
</template>

<script>

    export default {
        name: "QuizSettings",

        data () {
            return {
                isTime: false,
                isHard: false,
                userInput: false,
                number: 3,
            }
        },
        methods: {
            newQuiz (number) {
              if (number > 0) {
                this.$emit('start', number);
                this.userInput = false;
              }
            },
            numberInput () {
                this.userInput = true;
            }
        }
    }
</script>

<style scoped>
    .checkboxes {
        font-size: 1.5vw;
        margin-top: 2vw;
    }

    .checkboxes label {
        margin-right: 2vw;
    }

    .checkboxes input {
        margin-right: 1vw;
    }

    .settings button{
        font-family: 'Exo 2', sans-serif;
        width: 6vw;
    }

    .infinityMode {
        width: 15vw !important;
    }

    .userButton{
        width: 20vw !important;
    }

    form {
        margin-top: 1vw;
    }
   button.back {
     width: 8vw;
   }

    .submitNumber {
        padding: .5vw !important;
        font-size: 1.5vw;
        height: auto !important;
        width: auto !important;
    }
</style>