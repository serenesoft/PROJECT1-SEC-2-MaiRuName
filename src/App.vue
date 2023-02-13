<script>
export default {
  data() {
    return {
      previous: null,
      displayInput: '',
      displayValue: '',
      operator: null,
      operatorClicked: false
    }
  },
  methods: {
    clear() {
      this.displayInput = '';
      this.displayValue = '';
    },
    sign() {
      this.displayValue = this.displayValue.charAt(0) === '-' ? this.displayValue.slice(1) : `-${this.displayValue}`;
    },
    percent() {
      this.displayValue = `${parseFloat(this.displayValue)/100}`
    },
    // append(number) {
    //   if (this.operatorClicked) {
    //     this.displayValue = '';
    //     this.operatorClicked = false;
    //   }
    //   this.displayValue = `${this.displayValue}${number}`;
    // },
    append(number){
      if(!this.operatorClicked){
        this.displayInput += number;
      }
      else{
        this.operatorClicked = false
        this.displayInput = ''
        this.displayValue=''
        this.displayInput += number
      }
    },
    dot() {
      if (this.displayValue.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.displayValue;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => a/b;
      this.setPrevious();

    },
    times() {
      this.operator = (a, b) => a*b;
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => a-b;
      this.setPrevious();
    },
    plus() {
      this.operator = (a, b) => a+b;
      this.setPrevious();
    },
    equal() {
      this.displayValue = `${this.operator(parseFloat(this.displayValue), parseFloat(this.previous))}`;
    }
  }
}
</script>

<template>
  <div class="calculator">
    <div class="display">
        <div class="input">{{ displayInput }}</div>
        <div class="output">{{ displayValue || '0' }}</div>
    </div>
    <button @click="clear" class="buttons action">AC</button>
    <button @click="sign" class="buttons action">+/-</button>
    <button @click="percent" class="buttons action">%</button>
    <button @click="divide" class="buttons operator">÷</button>
    <button @click="append('7')" class="buttons">7</button>
    <button @click="append('8')" class="buttons">8</button>
    <button @click="append('9')" class="buttons">9</button>
    <button @click="times" class="buttons operator">x</button>
    <button @click="append('4')" class="buttons">4</button>
    <button @click="append('5')" class="buttons">5</button>
    <button @click="append('6')" class="buttons">6</button>
    <button @click="minus" class="buttons operator">-</button>
    <button @click="append('1')" class="buttons">1</button>
    <button @click="append('2')" class="buttons">2</button>
    <button @click="append('3')" class="buttons">3</button>
    <button @click="plus" class="buttons operator">+</button>
    <button @click="append('0')" class="buttons zero">0</button>
    <button @click="dot" class="buttons">.</button>
    <button @click="equal" class="buttons operator">=</button>
  </div>
</template>

<style scoped>
.calculator {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0 auto;
  width: 400px;
  font-size: 30px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  padding-top: 90px;
}
.display {
  grid-column: 1/5;
  //align-items: flex-end;
}
.input {
  background-color: #282828;
  height: 20px;
  color: white;
  text-align: right;
  padding-top: 5px;
  border-radius: 0px;
  font-size: 14px;
  //padding-right: 35px;
}
.output {
  background-color: #282828;
  color: white;
  text-align: right;
  font-size: 40px;
  font-weight: bold;
  padding-right: 35px;
}
.operators {
  color: orange;
}
.zero {
  grid-column: 1/3;
}
.buttons {
  background-color: #eee;
  border: 1px solid #333;
}
.operator {
  background-color: orange;
  color: white;
}
.action {
  background-color: #898989;
  color: #f2f2f2;
}
.history{
  grid-column: 1/5;
}
</style>
