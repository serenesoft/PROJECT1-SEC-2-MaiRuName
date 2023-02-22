!
<script setup>
  import { ref } from "vue";
    let current = ref('')
    let previousNumber = ref('')
    let equation = ref('') // เก็นสมการ
    let currentOperator = ref('') 
    let operator = ref(null)
    let operatorClicked = ref(false)
    let clickEqual = ref(false) 
  
    // ประวัติการคำนวณ
    let histories = ref([])
    let result = ref('')
    // ปุ่มปิดเปิด history
    let toggleHistory = ref(false) 

    function clear() {
      current.value = '';
      equation.value = '';
    }
    function sign() {
      if(parseFloat(current.value) > 0){ // ไม่ให้ 0 ติดลบ
        current.value = current.value.charAt(0) === '-'?  current.value.slice(1) : `-${current.value}`;
    }
  }
    function del() {
      current.value =  current.value.slice(0, -1);
    }
    function append(number) {
      clickEqual = false
      // แสดง ตัวเลข
      if (operatorClicked || current.value === '0' ) { // ไม่ให้กด 0 ซ้ำได้
         current.value = "";
         operatorClicked = false;
      }
      if(current.value.length > 10){ // ไม่ให้กดตัวเลขเกิน 10 ตัว
        return current.value
      }
       current.value = `${current.value}${number}`;
    }
  
    function dot() {
      if ( current.value.includes(".") ||  current.value === "") {
        return ;
      }
      if(current.value === '0'){
        append("0.");
      }else{
      append(".");
      }
    }
    function setPrevious() {
       previousNumber.value = current.value;
       operatorClicked = true;
    }
    function setEquation() {
       equation.value = `${ current.value}${ currentOperator}`;
    }
    function setResultAfterClickOperator() {
       result.value = `${current.value} ${currentOperator}`;
    }
    // เครื่องหมาย -----------------------------------------------
    function divide() {
      if (current.value === "") {  
    return;
      }
       operator = (a, b) => b / a;
       currentOperator = "÷";
       setEquation();
       setPrevious();
       setResultAfterClickOperator()
    }
    function multiply() {
      if (current.value === "") {  
    return;
      }
       operator = (a, b) => a * b;
       currentOperator = "x";
       setEquation();
       setPrevious();
       setResultAfterClickOperator()
    }
    function minus() {
      if (current.value === "") {  
    return;
      }
       operator = (a, b) => a - b;
       currentOperator = "-";
       setEquation();
       setPrevious();
       setResultAfterClickOperator()
    }
    function plus() {
      if (current.value === "") {  
      return;
      }
       operator = (a, b) => a + b;
       currentOperator = "+";
       setEquation();
       setPrevious();
       setResultAfterClickOperator()
    }
    function equal() {
      if (clickEqual) { // ดักไม่ให้กด = ซ้ำ
      return ;
      }else if(previousNumber.value.length !== 0){ // ถ้า previous ไม่มีค่าจะไม่ทำอันนี้ ดักไม่ให้กด = ก่อนกดเครื่องหมาย
       equation.value = `${ previousNumber.value}${ currentOperator}${ current.value} =`;
       result.value = result.value + ' ' +current.value
       current.value = operator(parseFloat(previousNumber.value), parseFloat(current.value));
          if(!Number.isInteger(current.value)){ // ถ้าcurrent ไม่ใช่จำนวนเต็ม ให้แปลงเป็นFloat ก่อน แล้วค่อยtoFixed() ตำแหน่งทศนิยม
            current.value = parseFloat(current.value).toFixed(5)
      }
    
    }
    result.value = result.value + ' = ' +current.value  // [History] หลังจากคำนวณ เอาresultอันก่อนหน้านี้มาใช้ต่อ จะได้ '3 + 5 = 8'
    histories.value.push(result.value) // [History] push result เข้าไปเก็บใน history
    previousNumber.value = null;
    clickEqual = true;
    }
    function clearHistory() {
      histories.value = []
    }
</script>

<template>
  <div class="flex min-h-screen bg-gray-400 items-center justify-center max-sm">
    <div class="bg-slate-50 p-10 rounded-lg m-10">
      <h1 class="text-5xl text-center pb-7 font-semibold">Calculator</h1>
      <div class="calculator text-3xl w-96 p-8">
        <div class="display bg-slate-800 col-span-4 pl-2 rounded-lg mb-1 text-right pr-2 relative overflow-hidden">
          <div class="text-gray-200 text-2xl">
            {{ equation }}
          </div>
          <div class="currentDisplay text-white text-5xl font-bold absolute bottom-1 right-2">
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
      <div @click="toggleHistory = !toggleHistory" class="historyBtn hover:ring-4 hover:ring-offset-2 ring-[#14081f]">
        <h1>HISTORY</h1>
      </div>
    </div>
    <div v-show="toggleHistory" class="bg-purple-900 pt-8 px-10 ml-7 rounded-lg">
      <h1 class="text-5xl text-center pb-7 font-semibold text-white">History</h1>
      <div class="bg-gray-200 rounded-lg overflow-y-auto w-96 h-96">
        <ul class="text-lg pl-3 pt-2">
            <li v-for="history in histories">{{ history }}</li>
        </ul>
      </div>
      <div class="clearHistoryBtn hover:ring-4 hover:ring-offset-2 ring-[#99de1e]">
        <h1 @click="clearHistory() ">Clear history</h1>
      </div>
    </div>
  </div>
</template>

<style scoped>
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 80px);
  grid-template-rows: repeat(6, 80px);
  user-select: none;
}

.display {
  grid-column: 1 /5;
  background-color: #333;
}
.zero {
  grid-column: 1 / 3;
}
.btn {
  color: white;
  background-color: #462870;
  border: 1px solid black;
  border-radius: 10px;
  text-align: center;
  margin: 3px;
  padding: 14px;
}
.operator {
  background-color: orange;
  color: white;
}
.btn:hover {
  background-color: rgb(186, 235, 255);
  color: black;
}
.btn:active {
  transform: scale(1.1, 1.1);
}
.historyBtn {
  background-color: #14081f;
  border: 1px solid black;
  color: white;
  font-size: x-large;
  font-weight: bold;
  border-radius: 10px;
  text-align: center;
  margin-top: 25px;
  padding: 14px;
  user-select: none;
}
.historyBtn:hover {
  transition-duration: 0.4s;
  background-color: #e2e8f0;
  color: #14081f;
}
.historyBtn:active{
  transform: scale(1.1, 1.1);
}
.clearHistoryBtn{
  background-color: #99de1e;
  border: 1px solid black;
  color: #341f50;
  font-weight: bold;
  border-radius: 10px;
  text-align: center;
  margin: 25px 30px;
  padding: 5px;
  user-select: none;
}
.clearHistoryBtn:hover{
  transition-duration: 0.4s;
  background-color: #e2e8f0;
  color: #341f50;
  border: 1px solid #99de1e;
}
.clearHistoryBtn:active{
  transform: scale(1.1, 1.1);
}
</style>
