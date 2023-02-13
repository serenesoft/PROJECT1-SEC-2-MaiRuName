!<script >
export default{
  data(){
    return{
      current: '0',
      equation:'',
      operaToEquation: null,
      previous:null,
      operator: null,
      operatorClicked:false,
    }
  },
  methods:{
    clear(){
      this.current = '0'
    },
    sign(){
      this.current = this.current.charAt(0) === '-'?this.current.slice(1): `-${this.current}`
    },
    percent(){ // ทำเป็น percent 
      this.current = `${parseFloat(this.current) / 100}`
    },
    append(number){ // แสดง ตัวเลข
      if(this.operatorClicked){
        this.current='0'
        this.operatorClicked=false
      }
      this.current = `${this.current}${number}`
    },
    dot(){
      if(this.current.indexOf('.') === -1){ // ถ้า not found ให้แสดง . ถ้ามี dot แล้วไม่สามารถใส่ซ้ำได้
        this.append('.')
      }
    },
    setPrevious(){
      this.previous = this.current
      this.operatorClicked=true
    },
    setEquation(){
      this.equation = `${this.current}${this.operaToEquation}`
    },
    // เครื่องหมาย -----------------------------------------------
    divide(){
      this.operator = (a,b) => a/b
      this.previous = this.current
      this.operatorClicked =true
      this.operaToEquation = '÷'
      this.setEquation()
      this.setPrevious()
    },
    multiply(){
      this.operator = (a,b) => a*b
      this.previous = this.current
      this.operatorClicked =true
      this.operaToEquation = 'x'
      this.setEquation()
      this.setPrevious()
    },
    minus(){
      this.operator = (a,b) => a-b
      this.previous = this.current
      this.operatorClicked =true
      this.operaToEquation = '-'
      this.setEquation()
      this.setPrevious()
    },
    plus(){
      this.operator = (a,b) => a+b
      this.previous = this.current
      this.operatorClicked =true
      this.operaToEquation = '+'
      this.setEquation()
      this.setPrevious()
    },
    equal(){
      this.equation = `${this.previous}${this.operaToEquation}${this.current}=`
      this.current = `${this.operator(parseFloat(this.current),parseFloat(this.previous))}`
      this.previous = null
      this.operaToEquation = '='
      
    },
    
  }
}
</script>
 
<template>
  <div class="calculator">
    <h1> {{ equation }}</h1>
    <div class="display text-end"><span class='mr-3 text-white'>{{ parseInt(current).toLocaleString() || '' }}</span></div>
    <div @click="clear" class="btn">C</div>
    <div @click="sign" class='btn'>+/-</div>
    <div @click="percent" class="btn">%</div>
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
    <div @click="append('0')" class="btn zero text-center">0</div>
    <div @click="dot()" class="btn">.</div>
    <div @click="equal()" class="btn operator">=</div>
  </div>
</template>
 
<style scoped>
.calculator{
  margin: 0 auto;
  width: 500px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(6, 1fr);
  user-select: none;
}

.display{
  grid-column: 1 /5;
  background-color: #333;
  
}
.zero{
  grid-column: 1 / 3
}
.btn{
  background-color: #eee;
  border: 1px solid #999;
  text-align: center
  
}
.operator{
  background-color: orange;
  color: white;
}
.btn:hover{
  background-color: rgb(186, 235, 255);
  color: black;
}
.btn:active{
  transform: scale(1.1,1.1);
}
</style>