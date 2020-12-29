<template>
    <div>
        <div class="numbers">
            <div class="generalNumber">
                <p>Всего вопросов отвечено правильно {{generalRightNumber}}/{{generalNumber}}</p>
                <p>Процент правильных ответов {{rightPercent}}%</p>
            </div>
            <div class="individualNumber">
                <p>Правильных ответов в сложении {{numbers[0].numberRight}}/{{numbers[0].generalNumber}}</p>
                <p>Правильных ответов в вычитании {{numbers[1].numberRight}}/{{numbers[1].generalNumber}}</p>
                <p>Правильных ответов в делении {{numbers[2].numberRight}}/{{numbers[2].generalNumber}}</p>
                <p>Правильных ответов в умножении {{numbers[3].numberRight}}/{{numbers[3].generalNumber}}</p>
                <p>Правильных ответов в смешанном режиме {{numbers[4].numberRight}}/{{numbers[4].generalNumber}}</p>
            </div>
        </div>
        <div class="achievements">
            <div class="achievement" v-for="(achievement, index) in achievements" :key="index">
                <p class="achievement_title">{{achievement.title}}</p>
                <p class="achievement_description" :class="{achievement_description_active: isHovering && index === hoveringIndex}">{{achievement.description}}</p>
                <img class="achievement_img"
                     :class="{active: achievement.status}"
                     @mouseover="hover(index)"
                     @mouseout="hover(index)"
                     :src="achievement.img">
            </div>
        </div>
        <button class="back" @click="$emit('onBack')">Назад</button>
    </div>
</template>

<script>
    export default {
        name: "Statistics",
        props: ["generalNumber", "generalRightNumber", "numbers", "achievements"],
        data () {
            return {
                isHovering: false,
                hoveringIndex: 1,
            }
        },
        computed: {
            rightPercent: function () {
                if (this.generalNumber !== 0)
                    return ((this.generalRightNumber/this.generalNumber) * 100).toFixed(2);
                else
                    return 0;

            }
        },
        methods: {
            hover(index) {
                this.isHovering = !this.isHovering;
                this.hoveringIndex = index;
            }
        }
    }
</script>

<style scoped>
    .individualNumber {
        display: flex;
    }

    .numbers {
        font-family: 'Exo 2', sans-serif;
        font-size: 1.5vw;
        display: flex;
        flex-direction: column;
    }
    .active {
        filter: opacity(100%) !important;
    }

    .achievements {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: flex-end;
        font-family: 'Exo 2', sans-serif;
    }

    .achievement {
        position: relative;
        margin-right: 2vw;
        width: 15vw;
    }

    .achievement_description {
        font-size: 1.5vw;
        top: 2vw;
        left: 5vw;
        z-index: 1;
        position:absolute;
        display: none;
        background-color: white;
        box-shadow: 6px 6px 2px 1px rgba(0, 0, 255, .2);
        width: 15vw;
        text-align: justify;
        padding: 10px;
    }

    .achievement_description_active {
        display: block !important;
    }

    .achievement_img {
        width: 10vw;
        filter: opacity(25%);
    }

</style>