<template>
  <div id="app" :class="{hoveringAdd: isHoveringAdd, hoveringSub: isHoveringSub, hoveringDiv: isHoveringDiv, hoveringMul: isHoveringMul,
  hoveringMix: isHoveringMix, hoveringStats: isHoveringStats}">
    <let-it-snow
        v-bind="snowConf"
        :show="show"
    ></let-it-snow>
    <h1 class="main_title">Математический тест</h1>
    <OperatorQuiz @end="changeNumber"
                  @onBack="clearOperator"
                  v-if="operator"
                  :operator="operator"
                  :achievements="achievements"
                  :generalRightNumber="generalRightNumber"
                  :lordOfElements="lordOfElements"/>
    <OperatorSelector v-if="!operator && !isStats"
                      @changeOperator="changeOperator"
                      @showStats = "showStats"
                      @changeHover = "hoverChange"/>
    <Statistics v-if="isStats"
                @onBack="clearOperator"
                :generalNumber="generalNumber"
                :generalRightNumber="generalRightNumber"
                :numbers="numbers"
                :achievements="achievements"/>
  </div>
</template>

<script>
import OperatorSelector from "./components/OperatorSelector";
import OperatorQuiz from "./components/OperatorQuiz";
import Statistics from "./components/Statistics";

export default {
  name: 'App',
  data () {
    return {
      snowConf: {
        windPower : 2,
        speed : 3,
        count : 25,
        size : 10,
        opacity : 1,
        images: ['https://raw.githubusercontent.com/bob-chen/let_it_snow/master/demo/snow.png']
      },
      show: false,
      lordOfElements: [
        { operator: '+', status: false},
        { operator: '-', status: false},
        { operator: '/', status: false},
        { operator: '*', status: false},
      ],
      achievements: [
        { id: 1, title: 'Повелитель стихий', status: false,
          description: 'Завершите хотя бы один тест без ошибок в каждой секции.', img: require('./assets/achievement3.png') },
        { id: 2, title: 'Ты гений?!', status: false,
          description: 'Ответье правильно на 50 вопросов.', img: require('./assets/achievement2.png')  },
        { id: 3, title: 'Начало пути', status: false,
          description: 'Завершите хотя бы один тест без ошибок.', img: require('./assets/achievement1.png') },
        { id: 4, title: 'Сорвиголова', status: false,
          description: 'Попробуйте режим хардкора.', img: require('./assets/achievement5.png') },
        { id: 5, title: 'Я даже не успел моргнуть', status: false,
          description: 'Попробуйте скоростной режим.', img: require('./assets/achievement4.png') }
      ],
      isHoveringAdd: false,
      isHoveringSub: false,
      isHoveringDiv: false,
      isHoveringMul: false,
      isHoveringMix: false,
      isHoveringStats: false,
      isStats: false,
      operator: null,
      generalNumber: 0,
      generalRightNumber: 0,
      numbers: [
        { operator: '+',  numberRight: 0,  generalNumber: 0},
        { operator: '-',  numberRight: 0,  generalNumber: 0},
        { operator: '/',  numberRight: 0,  generalNumber: 0},
        { operator: '*',  numberRight: 0,  generalNumber: 0},
        { operator: '&',  numberRight: 0,  generalNumber: 0},
      ],
    }
  },
  mounted () {
    this.show = true
  },
  methods: {
    hoverChange(bool, operator) {
      switch(operator) {
        case '+':
          this.isHoveringAdd = bool;
          break;
        case '-':
          this.isHoveringSub = bool;
          break;
        case '*':
          this.isHoveringMul = bool;
          break;
        case '/':
          this.isHoveringDiv = bool;
          break;
        case '&':
          this.isHoveringMix = bool;
          break;
        case '?':
          this.isHoveringStats = bool;
          break;
      }
    },
    showStats () {
      console.log(this.generalNumber);
      this.isStats = true;
    },
    changeNumber (counter, wholeNumber, operator) {
      this.generalNumber += wholeNumber;
      this.generalRightNumber += counter;
      switch(operator) {
        case '+':
          this.numbers[0].numberRight += counter;
          this.numbers[0].generalNumber += wholeNumber;
          break;
        case '-':
          this.numbers[1].numberRight += counter;
          this.numbers[1].generalNumber += wholeNumber;
          break;
        case '*':
          this.numbers[3].numberRight += counter;
          this.numbers[3].generalNumber += wholeNumber;
          break;
        case '/':
          this.numbers[2].numberRight += counter;
          this.numbers[2].generalNumber += wholeNumber;
          break;
        case '&':
          this.numbers[4].numberRight += counter;
          this.numbers[4].generalNumber += wholeNumber;
          break;
      }
    },
    changeOperator(operator) {
      this.operator = operator;
    },
    clearOperator () {
      this.operator = null;
      this.isStats = false;
      this.isHoveringAdd = false;
      this.isHoveringSub = false;
      this.isHoveringDiv = false;
      this.isHoveringMul = false;
      this.isHoveringMix = false;
      this.isHoveringStats = false;
    }
  },
  components: { OperatorSelector, OperatorQuiz, Statistics }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Exo+2:ital,wght@0,300;0,500;0,600;1,300&display=swap');
  .main_title {
    margin-bottom: .5vw !important;
  }

  .hoveringAdd {
    background-image: url("assets/christmas/add.png");
    background-size: cover;
  }
  .hoveringSub {
    background-image: url("assets/christmas/sub.png");
    background-size: cover;
  }
  .hoveringDiv {
    background-image: url("assets/christmas/div.png");
    background-size: cover;
  }
  .hoveringMul {
    background-image: url("assets/christmas/mul.png");
    background-size: cover;
  }
  .hoveringMix {
    background-image: url("assets/christmas/mix.png");
    background-size: cover;
  }
  .hoveringStats{
    background-image: url("assets/christmas/stats.png");
    background-size: cover;
  }
#app {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  font-family: 'Exo 2', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  font-size: 2vw;
}

  button {
    transition: all .1s linear;
    border: none;
    border-radius: 3px;
    margin-right: 2vw;
    padding: 1vw;
    width: 20vw;
    height: 80px;
    background-color: indianred;
    color: white;
    font-size: 24px;
    cursor: pointer;
    vertical-align: top;
  }


  button:hover {

    transform: scale(1.1);
  }
</style>
