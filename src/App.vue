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
      this.displayInput = this.displayInput.charAt(0) === '-' ? this.displayInput.slice(1) : `-${this.displayInput}`;
    },
    percent() {
      this.displayValue = `${parseFloat(this.displayInput)/100}`
    },
    // append(number) {
    //   if (this.operatorClicked) {
    //     this.displayValue = '';
    //     this.operatorClicked = false;
    //   }
    //   this.displayValue = `${this.displayValue}${number}`;
    // },
    append(key){
      if(!this.operatorClicked){
        this.displayInput += key;
      }
      else{
        this.operatorClicked = false
        this.displayInput = ''
        this.displayValue=''
        this.displayInput += key
      }
    },
    dot() {
      if (this.displayInput.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.displayInput;
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
      // this.displayInput = `${this.operator(parseFloat(this.displayInput), parseFloat(this.previous))}`;
      if(this.displayInput.includes('%')){
        const arr = this.displayInput.split('')
        const newArr = []
        for(let i=0;i<arr.length;i++){
          if(arr[i]!='%'){
            newArr.push(arr[i])
          }
          else{
            newArr.push('/100')
          }
        }
        this.displayInput = newArr.join('')
        console.log(newArr)
      }
      console.log(this.displayInput)
      this.displayValue = eval(this.displayInput)
      this.operatorClicked = true
    }
    }
}
</script>

<template>
  <div class="app">
    <div class="calculator">
      <div class="display">
        <div class="input">{{ displayInput }}</div>
        <div class="output">{{ displayValue || '0' }}</div>
      </div>
      <button @click="clear" class="buttons action">AC</button>
      <button @click="sign" class="buttons action">+/-</button>
      <button @click="percent" class="buttons action">%</button>
      <button @click="append('/')" class="buttons operator">÷</button>
      <button @click="append('7')" class="buttons">7</button>
      <button @click="append('8')" class="buttons">8</button>
      <button @click="append('9')" class="buttons">9</button>
      <button @click="append('*')" class="buttons operator">x</button>
      <button @click="append('4')" class="buttons">4</button>
      <button @click="append('5')" class="buttons">5</button>
      <button @click="append('6')" class="buttons">6</button>
      <button @click="append('-')" class="buttons operator">-</button>
      <button @click="append('1')" class="buttons">1</button>
      <button @click="append('2')" class="buttons">2</button>
      <button @click="append('3')" class="buttons">3</button>
      <button @click="append('+')" class="buttons operator">+</button>
      <button @click="append('0')" class="buttons zero">0</button>
      <button @click="append('.')" class="buttons">.</button>
      <button @click="equal" class="buttons operator">=</button>
    </div>
  </div>
</template>

<style scoped>
.app {
  background-color: #1a0e25;
  width: 100%;
  font-family: Helvetica, Arial, sans-serif;
}
.calculator {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0 auto;
  width: 600px;
  font-size: 20px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  padding-top: 170px;
  padding-bottom: 25%;
}
.display {
  grid-column: 1/5;
  //align-items: flex-end;
}
.input {
  background-color: #14081f;
  height: 20px;
  color: white;
  text-align: right;
  padding-top: 5px;
  border-radius: 0px;
  font-size: 14px;
  //padding-right: 35px;
}
.output {
  background-color: #14081f;
  color: white;
  text-align: right;
  font-size: 40px;
  font-weight: bold;
  padding-right: 35px;
}
.operators {
  color: #99de1e;
}
.zero {
  grid-column: 1/3;
}
.buttons {
  background-color: #eee;
  border: 1px solid #333;
}
.operator {
  background-color: #99de1e;
  color: white;
}
.action {
  background-color: #7554a3;
  color: #f2f2f2;
}
</style>
