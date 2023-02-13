<script>
export default {
  data() {
    return {
      previous: null,
      current: '',
      operator:null,
      operatorClicked: false,
      history: ''
    }
  },
  methods:{
    clear(){
      this.current = ''
    },
    sign(){
      this.current = this.current.charAt(0) === '-' ?  this.current.slice(1): `-${this.current}`
    },
    setPrevious(){
      this.previous=this.current 
      this.operatorClicked = true
    },
    percent(){
      this.current = `${parseFloat(this.current) / 100}`
    },
    divide(){
      this.operator = (a,b) => a/b
      this.setPrevious()
    }
    ,
    multiply(){
      this.operator = (a,b) => a*b
      this.setPrevious()
    },
    minus(){
      this.operator = (a,b) => a-b
      this.setPrevious()
    },
    plus(){
      this.operator = (a,b) => a+b
      this.setPrevious()
    },
    equal(){
      this.current = `${this.operator(parseFloat(this.previous),parseFloat(this.current))}`
      this.previous = null
    },
    append(number){
      if(this.operatorClicked){
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`
    },
    dot(){
      if(this.current.indexOf('.') === -1)
      this.append('.')
    },
  }
}
</script>

<template>
<div class="bg-yellow-400 flex justify-center" >
  <div class="calculator">
    <div class="display">{{current || '0'}}</div>
    <div @click="clear" class="btn">C</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn , operator">÷</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="multiply" class="btn , operator">x</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn , operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="plus" class="btn , operator">+</div>
    <div @click="append('0')" class="btn , zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn , operator">=</div>
  </div>
</div>
</template>

<style scoped>
.calculator {
  margin: auto;
  width: 500px;
  padding: 30px;
  border-radius: 25px;
  background-color: #14091f;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 10fr);
  grid-auto-rows: minmax(50px, auto);

}

.display {
  grid-column: 1/5;
  background-color: #7554a3;
  border-radius: 10px;
  color: white;
  text-align: center;
  padding: 10px;
  margin-bottom: 10px;
}

.zero {
  grid-column: 1/3;
}

.btn {
  background-color: #381f50;
  border: 0px solid #999;
  padding: 10px;
  margin: 3px;
  border-radius: 10px;
  text-align: center;
  color:white;
}
.btn:hover{
  cursor: pointer;
  background: #99de1e;
  color: #14091f;
}

.operator {
  background-color: #99de1e;
  color: #14091f;
}
.operator:hover{
  cursor: pointer;
  background: #984de7;
  color: white;
}
</style>
