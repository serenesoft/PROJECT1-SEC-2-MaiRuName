<script>
export default {
  data() {
    return {
      current: '',
      previousNumber: '',
      operator: null,
      currentOperator: null,
      operatorClicked: false,
      equation: ''
    }
  },
  methods: {
    clear() {
      this.current = ''
      this.equation = ''
    },
    sign() {
      this.current = this.current.charAt(0) === "-" ?
      this.current.slice(1) : `-${this.current}`
    },
    del() {
      this.current = `${this.current.slice(0, -1)}`
    },
    append(number) {
      if(this.operatorClicked) {
        this.current = ''
        this.operatorClicked = false
      }
      this.current = `${this.current}${number}`
    },
    setPreviousNumber() {
      this.previousNumber = this.current
      this.operatorClicked = true
    },
    dot() {
      if(this.current.includes('.') || this.current === ''){ // ดักไม่ให้กด'.'ซ้ำและกดตัวแรก
        return ;
      }else {
        this.append('.')
      }
    },
    divide(){
      this.operator = (a, b) => a / b
      this.currentOperator = '÷'
      this.setEquation()
      this.setPreviousNumber()
    },
    multiply() {
      this.operator = (a, b) => a * b
      this.currentOperator = 'x'
      this.setEquation()
      this.setPreviousNumber()
    },
    minus() {
      this.operator = (a, b) => a - b
      this.currentOperator = '-'
      this.setEquation()
      this.setPreviousNumber()
    },
    plus() {
      this.operator = (a, b) => a + b
      this.currentOperator = '+'
      this.setEquation()
      this.setPreviousNumber()
    },
    setEquation() {
      this.equation = `${this.current} ${this.currentOperator}`
    },
    equal() {
      this.equation = `${this.previousNumber} ${this.currentOperator} ${this.current} = `
      this.current = this.operator(parseFloat(this.previousNumber), parseFloat(this.current))
      this.previousNumber = null
    }
  }
}
</script>
 
<template>
  
<div class="bg-gray-400 flex items-center justify-center h-screen">
  <div class="bg-slate-50 p-10 rounded-lg ">
    <h1 class="text-5xl text-center pb-7 font-semibold">Calculator</h1>
    <div class="calculator text-3xl w-96 p-8">
      <div class="display bg-slate-800  col-span-4 pl-2 rounded-lg mb-1 text-right pr-2 relative">
        <div class="text-gray-200 text-2xl ">
          {{ equation }}
        </div>
        <div class="text-white text-5xl font-bold absolute bottom-1 right-2">
          {{ current || 0 }}
        </div>
      </div>
      <div @click="clear()" class="btn">C</div>
      <div @click="sign()" class="btn">+/-</div>
      <div @click="del()" class="btn">←</div>
      <div @click="divide()" class="btn operator">÷</div>
      <div @click="append('7')" class="btn">7</div>
      <div @click="append('8')" class="btn">8</div>
      <div @click="append('9')" class="btn">9</div>
      <div @click="multiply()" class="btn operator">x</div>
      <div @click="append('4')" class="btn">4</div>
      <div @click="append('5')" class="btn">5</div>
      <div @click="append('6')" class="btn">6</div>
      <div @click="minus()" class="btn operator">-</div>
      <div @click="append('1')" class="btn">1</div>
      <div @click="append('2')" class="btn">2</div>
      <div @click="append('3')" class="btn">3</div>
      <div @click="plus()" class="btn operator">+</div>
      <div @click="append('0')" class="btn col-span-2">0</div>
      <div @click="dot()" class="btn">.</div>
      <div @click="equal()" class="btn operator">=</div>
    </div>
  </div>
</div>
</template>
 
<style scoped>
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 80px);
  grid-template-rows: repeat(6, 80px);
  background-color: #e5e7eb;
  border-radius: 5%;
  user-select: none;
}
.btn {
  background-color: #cbd5e1;
  border: 1px solid black;
  border-radius: 10px;
  text-align: center;
  margin: 3px;
  padding: 14px;
}
.btn:hover {
  transition-duration: 0.4s;
  background-color:  #94a3b8;
  color: white;
}
.operator {
  background-color: orange;
  color: white;
}
.operator:hover {
  background-color: rgb(250, 200, 109);
  color: black;
}
</style>