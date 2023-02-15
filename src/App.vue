<script setup>
import { ref } from 'vue';
let current = ref('');
let previousNumber = ref('');
let operator = ref(null);
let currentOperator = ref(null);
let operatorClicked = ref(false); 
let equation = ref(''); // เอาไว้เก็บสมการ(ที่ขึ้นตัวๆเล็กด้านบนอะ)
let clickedEqual = ref(false) // ถ้ากด = แล้วมันจะเปลี่ยนเป็น true | พอเป็น true แล้วจะไปดักไม่ให้กด = ซ้ำ (บรรทัด98) | แล้วไปเซ็ตค่ากลับเป็น false เวลากดปุ่มตัวเลขที่function append

function clear() {
  current.value = '';
  equation.value = '';
}

function sign() {
  current.value =
    current.value.charAt(0) === "-" ? current.value.slice(1) : `-${current.value}`;
}

function del() {
  current.value = `${current.value.slice(0, -1)}`;
}

function append(number) {
  clickedEqual = false;
  if (operatorClicked) {
    current.value = "";
    operatorClicked = false;
  }
  current.value = `${current.value}${number}`;
}

function setPreviousNumber() {
  previousNumber.value = current.value; // เปลี่ยนตัวเลขcurrent --> previous (ตัวเลขตัวแรก)  -->A<-- + B = X
  operatorClicked = true; // เมื่อกดเครื่องหมายจะเปลี่ยนเป็น true แล้วไปเข้าเงื่อนไขใน function append
}

function dot() {
  if (current.value.includes(".") || current.value === "") {
    // ดักไม่ให้กด'.'ซ้ำและกดตัวแรก
    return;
  } else {
    append(".");
  }
}

function divide() {
  if (current.value === "") {
    // ดักไม่ให้กดเครื่องหมายเป็นตัวแรก
    return;
  } else {
    operator = (a, b) => a / b;
    currentOperator = "÷";
    setEquation();
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
    setPreviousNumber();
  }
}

function setEquation() {
  equation.value = `${current.value} ${currentOperator}`; // แสดงสมการหลังจากที่กดเครื่องหมาย Ex. '3 +'
}

function equal() {
  if (clickedEqual) { // ดักไม่ให้กด = ซ้ำ
    return;
  } else {
    equation.value = `${previousNumber.value} ${currentOperator} ${current.value} = `; // แสดงสมการหลังจากที่กด '=' Ex. '3 + 5 ='
    current.value = operator(
      parseFloat(previousNumber.value),
      parseFloat(current.value)
    );
    previousNumber.value = null;
    clickedEqual = true;
  }
}
</script>
 
<template>
  <div class="bg-gray-400 flex items-center justify-center h-screen max-sm">
    <div class="bg-slate-50 p-10 rounded-lg">
      <h1 class="text-5xl text-center pb-7 font-semibold">Calculator</h1>
      <div class="calculator text-3xl w-96 p-8">
        <div
          class="display bg-slate-800 col-span-4 pl-2 rounded-lg mb-1 text-right pr-2 relative">
          <div class="text-gray-200 text-2xl">
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
  background-color: #e2e8f0;

}
.btn:active {
  transform: scale(1.2, 1.2);
}
.operator {
  background-color: orange;
  color: white;
}
.operator:hover {
  background-color: #fdba74;
  color: black;
}
</style>
