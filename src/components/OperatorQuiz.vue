<template>
    <div>
        <div class="set_achievements">
            <div v-if="notification" class="achievement_notification">
                <p>{{currentAchievement.title}}</p>
                <img class="achievement_img"
                     :src="currentAchievement.img">
            </div>
        </div>
        <div class="end" v-if="end">
            <div v-if="this.wholeNumber === 0">
                <h5>Вы правильно ответили на {{counter}} вопросов</h5>
            </div>
            <div v-if="this.wholeNumber !== 0">
                <h5>Вы правильно ответили на {{counter}}/{{wholeNumber}} вопросов</h5>
            </div>
            <h4> Еще раз? </h4>
        </div>
        <div v-if="isQuizStarted">
            <Timer v-if="isTime" @endTimer="endQuiz" :wholeNumber="wholeNumber"/>
            <h4>{{operandLeft}} {{currentOperator}} {{operandRight}}</h4>
            <div v-if="this.wholeNumber !== 0">
                <h5>Вы правильно ответили на {{counter}}/{{wholeNumber}} вопросов</h5>
            </div>
            <div v-if="this.wholeNumber === 0">
                <h5>Вы правильно ответили на {{counter}} вопросов</h5>
            </div>
            <button class="answer" v-bind:class="{ selected: index === correctIndex, mistake: isMistake, right: isRight }"
                    @click="selectAnswer(answer)" v-for="(answer, index) of answers" :key="index">{{answer}}</button>

            <button class="end" @click="endQuiz(true)">Закончить</button>
        </div>
        <QuizSettings
                :isTime="isTime"
                :isHard="isHard"
                @start="newQuizSet"
                @onBack="$emit('onBack')"
                @changeHard="changeHard"
                @changeTime="changeTime"
                v-if="!isQuizStarted"/>

    </div>
</template>

<script>
    import QuizSettings from "@/components/QuizSettings";
    import Timer from "@/components/Timer"
    export default {
        name: "OperatorQuiz",
        components: {QuizSettings, Timer},
        props: ["operator", "achievements", "generalNumber", "lordOfElements"],
        data () {
            return {
                isTime: false,
                isHard: false,
                currentAchievement: null,
                notification: false,
                isQuizStarted: false,
                operandLeft: null,
                operandRight: null,
                answers: [],
                expectedAnswer: null,
                counter: 0,
                number: 0,
                wholeNumber: 0,
                end: false,
                currentOperator: this.operator,
                isMistake: false,
                isRight: false,
                correctIndex: 1,
                activeAchievements: [],
            };
        },
        methods: {
            // changeQuestions() {
            //   if (this.counter % 10 === 1 && this.counter % 100 !== 11) {
            //       this.questions = "вопрос";
            //       console.log('я поменял окончание');
            //   } else if (this.counter % 10 > 1 && this.counter % 10 <= 4 && this.counter % 100 < 14) {
            //       this.questions = "вопроса";
            //       console.log('я поменял окончание');
            //   } else
            //       this.questions = "вопросов";
            // },
            changeTime(boolean) {
                this.isTime = boolean;
            },
            changeHard(boolean) {
              this.isHard = boolean;
            },
            startNotification() {
                this.currentAchievement = this.achievements[this.activeAchievements[0]];
                this.notification = true;
                setTimeout(() => {
                        this.notification = false;
                }, 3000);
                this.activeAchievements.shift();
                if (this.activeAchievements.length > 0)
                    setTimeout(() => {
                        this.startNotification();
                    }, 3000);
            },
            updateAchievements () {
                if (this.achievements[4].status === false && this.isTime) {
                    this.achievements[4].status = true;
                    this.activeAchievements.push(4);
                }

                if (this.achievements[3].status === false && this.isHard) {
                    this.achievements[3].status = true;
                    this.activeAchievements.push(3);
                }

                if (this.achievements[2].status === false && this.counter === this.wholeNumber && this.wholeNumber !== 0) {
                    this.achievements[2].status = true;
                    this.activeAchievements.push(2);
                }

                if (this.achievements[1].status === false && (this.generalRightNumber + this.counter) >= 50) {
                    this.achievements[1].status = true;
                    this.activeAchievements.push(1);
                }


                if (this.achievements[0].status === false) {
                    let flag = true;
                    for (let i = 0; i < this.lordOfElements.length; i++) {
                        let check = this.lordOfElements[i];
                        if (this.counter === this.wholeNumber && this.operator === check.operator) {
                            check.status = true;
                        }
                        if (check.status === false) {
                            flag = false;
                        }
                    }
                        if (flag) {
                            this.achievements[0].status = true;
                            this.activeAchievements.push(0);
                        }
                    }
                if (this.activeAchievements.length > 0)
                  this.startNotification();
            },
            endQuiz (bool) {
                if (bool && this.number !== Infinity && !this.isTime) {
                    this.wholeNumber = this.wholeNumber - this.number;
                }
                this.isQuizStarted = false;
                this.end = true;
                this.$emit('end', this.counter, this.wholeNumber, this.operator);
                this.updateAchievements();
                this.isHard = false;
                this.isTime = false;
            },
            newQuizSet (number) {
              this.number = 0;
              this.wholeNumber = 0;

                if (number != Infinity) {
                    this.wholeNumber = parseInt(number);
                    this.number = parseInt(number);
                } else {
                    this.number = Infinity;
                }
                this.end = false;
                this.counter = 0;
                this.startQuiz();
            },
            selectAnswer (answerSelected) {
                if (this.number === Infinity)
                    this.wholeNumber += 1;
                this.number--;
                if (answerSelected !== this.expectedAnswer) {
                    this.isMistake = true;
                    if (this.number === 0) {
                        setTimeout(() => this.endQuiz(), 500);
                    } else {
                        setTimeout(() => this.startQuiz(), 500);
                    }
                } else {
                    this.isRight = true;
                    this.counter++;
                    // if (this.number === Infinity)
                    //     this.changeQuestions();
                    if (this.number === 0) {
                        setTimeout(() => this.endQuiz(), 500);
                    }
                    else
                        setTimeout(() => this.startQuiz(), 500);
                }
            },
            startQuiz () {
                this.isMistake = false;
                this.isRight = false;
                this.isQuizStarted = true;

                if (this.isHard) {
                    this.operandLeft = parseInt(Math.random() * 50);
                    this.operandRight = parseInt(Math.random() * 50);
                } else {
                    this.operandLeft = parseInt(Math.random() * 13);
                    this.operandRight = parseInt(Math.random() * 13);
                }

                const methods = {
                    '+': (a, b) => a + b,
                    '-': (a, b) => a - b,
                    '/': (a, b) => (a / b).toFixed(1),
                    '*': (a, b) => a * b,
                };

                let keysMethods = Object.keys(methods);
                let methodToUse;

                if (this.operator === '&') {
                    methodToUse =  methods[keysMethods[Math.floor(keysMethods.length * Math.random())]];
                    this.currentOperator = Object.keys(methods).find(key => methods[key] === methodToUse);
                } else {
                    methodToUse = methods[this.operator];
                }

                this.answers = [];
                this.answers.push(methodToUse(this.operandLeft, this.operandRight + 2));
                this.answers.push(methodToUse(this.operandLeft + 1, this.operandRight));
                this.answers.push(methodToUse(this.operandLeft + 3, this.operandRight + 1));
                this.answers.push(methodToUse(this.operandLeft - 3, this.operandRight + 1));
                this.answers.push(methodToUse(this.operandLeft - 4, this.operandRight - 1));
                const expectedAnswer =  methodToUse(this.operandLeft, this.operandRight);



                this.correctIndex = Math.floor(Math.random() * 1000) % 5;
                this.answers[this.correctIndex] = expectedAnswer;
                this.expectedAnswer = expectedAnswer;

            },
        },
    };
</script>

<style scoped>
    .set_achievements {
        position: absolute;
        display: flex;
        flex-direction: column;
        right: 2vw;
    }
    .achievement_notification {
        font-family: 'Exo 2', sans-serif;
        position:relative;
        background-color: white;
        box-shadow: 6px 6px 2px 1px rgba(0, 0, 255, .2);
        padding: 1vw;
        z-index: 10;
        border: .5px lightgrey solid;
        display: flex;
        align-items: center;
    }

    .achievement_notification p {
        margin: 1vw;
        font-size: 1.5vw;
    }

    .achievement_notification img {
        width: 5vw;
    }
    .selected.mistake {
        background-color: darkred !important;
    }

    .selected.right {
        background-color: darkgreen;
    }

    .answer {
        font-family: 'Exo 2', sans-serif;
        width: 5vw;
    }
</style>