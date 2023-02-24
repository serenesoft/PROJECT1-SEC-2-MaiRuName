<script setup>
import { ref } from 'vue';
let current = ref('');
let previousNumber = ref('');
let operator = ref(null);
let currentOperator = ref('');
let operatorClicked = ref(false);
let equation = ref(''); // เอาไว้เก็บสมการ(ที่ขึ้นตัวๆเล็กด้านบน)
let clickedEqual = ref(false) // ถ้ากด = แล้วมันจะเปลี่ยนเป็น true | พอเป็น true แล้วจะไปดักไม่ให้กด = ซ้ำ | แล้วไปเซ็ตค่ากลับเป็น false เวลากดปุ่มตัวเลขที่ function append

// History
let histories = ref([])
let result = ref('')
// ปุ่ม toggle history
let toggleHistory = ref(false)

function clear() {
  current.value = '';
  equation.value = '';
}

function sign() {
  if(parseFloat(current.value) > 0 || parseFloat(current.value) < 0) { // ไม่ให้ 0 ติดลบ
    current.value = current.value.charAt(0) === '-'?  current.value.slice(1) : `-${current.value}`;
  }
}

function del() {
  current.value = `${current.value.slice(0, -1)}`;
}

function percent() {
  if (current.value === "") {
    return;
  } else if (current.value == 0){ //ดักไม่ให้กดเมื่อมีค่าเป็น 0 และกดเปอร์เซ็นต์  //ที่ใช้ = สองตัวเพราะว่าต้องการให้เช็คว่ามีค่าเท่ากับ 0 ไหม โดยไม่เช็ค Type
    current.value = 0;
  } else {
    setPreviousNumber();
    current.value = parseFloat(current.value/100).toFixed(5)
    currentOperator = "%";
    setEquationPercent();
    setResultAfterClickPercent();
  }
}

function append(number) {
  clickedEqual = false;
  if (operatorClicked || current.value === 0) { // ไม่ให้กด 0 ซ้ำได้
    current.value = "";
    operatorClicked = false;
  }
  if(current.value.length > 10) { // ไม่ให้กดตัวเลขเกิน 10 ตัว
    return current.value
  }
  if (current.value === 0) {
    return current.value = number
  }
  current.value = `${current.value}${number}`;
}

function setPreviousNumber() {
  previousNumber.value = current.value; // เปลี่ยนตัวเลข current --> previous (ตัวเลขตัวแรก)  -->A<-- + B = X
  operatorClicked = true; // เมื่อกดเครื่องหมายจะเปลี่ยนเป็น true แล้วไปเข้าเงื่อนไขใน function append
}

function setResultAfterClickOperan() {
  result.value = `${current.value} ${currentOperator}`; // [History] ตั้งค่าสมการที่จะเก็บไว้ใน history เก็บไว้ในตัวแปร result Ex.'3 +'
}

function setResultAfterClickPercent() {
  result.value = `${previousNumber.value} ${currentOperator}`;
  result.value = result.value + ' ' + '=' + ' ' + current.value
  histories.value.push(result.value);
}

function dot() {
  if (current.value.includes(".") || current.value === "") { // ดักไม่ให้กด'.'ซ้ำและกดตัวแรก
    return;
  } if(current.value === '0') {
    append("0.");
  }else {
    append(".");
  }
}

function setEquation() {
  equation.value = `${current.value} ${currentOperator}`; // [Equation] แสดงสมการหลังจากที่กดเครื่องหมาย Ex. '3 +'
}

function setEquationPercent(){
  equation.value = `${previousNumber.value} ${currentOperator} = `
}

// operator
function divide() {
  if (current.value === "") {  // ดักไม่ให้กดเครื่องหมายเป็นตัวแรก
    return;
  } else {
    operator = (a, b) => a / b;
    currentOperator = "÷";
    setEquation();
    setResultAfterClickOperan();
    setPreviousNumber();
  }
}

function multiply() {
  if (current.value === "") {
    return;
  } else {
    operator = (a, b) => a * b;
    currentOperator = "x";
    setEquation();
    setResultAfterClickOperan();
    setPreviousNumber();
  }
}

function minus() {
  if (current.value === "") {
    return;
  } else {
    operator = (a, b) => a - b;
    currentOperator = "-";
    setEquation();
    setResultAfterClickOperan();
    setPreviousNumber();
  }
}

function plus() {
  if (current.value === "") {
    return;
  } else {
    operator = (a, b) => a + b;
    currentOperator = "+";
    setEquation();
    setResultAfterClickOperan();
    setPreviousNumber();
  }
}

function equal() {
  if (clickedEqual) { // ดักไม่ให้กด = ซ้ำ
    return;
  } else if (previousNumber.value === '' || previousNumber.value === null) { // ดักไม่ให้กด = ก่อนกดเครื่องหมาย โดยเช็คว่า previous มีค่าหรือไม่
    return current.value;
  } else {
    equation.value = `${previousNumber.value} ${currentOperator} ${current.value} = `; // [Equation] แสดงสมการหลังจากที่กด '=' Ex. '3 + 5 ='
    result.value = result.value + ' ' +current.value // [History] ก่อนจะคำนวณ เอา result จาก setResultAfterClickOperan()มาใช้ต่อ จะได้ '3 + 5'
    current.value = operator(parseFloat(previousNumber.value), parseFloat(current.value));
    if(!Number.isInteger(current.value)) { // ถ้า current ไม่ใช่จำนวนเต็ม ให้แปลงเป็นFloat ก่อน แล้วค่อยtoFixed() ตำแหน่งทศนิยม
      current.value = parseFloat(current.value).toFixed(5)
    }
  }
    result.value = result.value + ' = ' +current.value // [History] หลังจากคำนวณ เอา result อันก่อนหน้านี้มาใช้ต่อ จะได้ '3 + 5 = 8'
    histories.value.push(result.value) // [History] push result เข้าไปเก็บใน history
    previousNumber.value = null;
    clickedEqual = true;
    console.log(typeof(current));
}

function clearHistory() {
  histories.value = []
}

</script>

<template>
  <div class="flex min-h-screen bg-[#14081f]  items-center justify-center max-sm select-none">
    <div class="bg-purple-900 p-10 rounded-lg m-10">
      <h1 class="text-5xl text-center pb-7 font-semibold text-white">Calculator</h1>
      <div class="calculator bg-[#14081f] text-3xl w-96 p-8">
        <div class="display bg-[#301652] col-span-4 pl-2 rounded-lg mb-1 text-right pr-2 relative overflow-hidden ">
          <div class="text-gray-200 text-2xl">
            {{ equation }}
          </div>
          <div class="currentDisplay text-white text-5xl font-bold absolute bottom-1 right-2 ">
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
        <div @click="append('0')" class="btn">0</div>
        <div @click="percent()" class="btn">%</div>
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
  border-radius: 5%;
  user-select: none;
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
.btn:hover {
  color: #341f50;
  transition-duration: 0.4s;
  background-color: #e2e8f0;
}
.btn:active {
  transform: scale(1.2, 1.2);
}
.operator {
  background-color: #99de1e;
  color: #341f50;
}
.operator:hover {
  background-color: #e2e8f0;
  color: #341f50;
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
