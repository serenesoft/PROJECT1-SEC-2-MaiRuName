<script>
export default {
  name: 'Calculator',
  props: {
    msg: String
  },
  data() {
    return {
      calculatorValue: '',
      calculatorElements: ['C', 'X', '÷', '-', 7, 8, 9, '+', 4, 5, 6, '%', 1, 2, 3, '=', 0, '.'],
      operator: null,
      previousCalculatorValue: '',
    }
  },

  methods: {
    action(n) {

      // Append Value
      if (!isNaN(n) || n === '.') {
        this.calculatorValue += n + '';
      }

      // Clear value
      if (n === 'C') {
        this.calculatorValue = '';
      }

      // Percentage
      if (n === '%') {
        this.calculatorValue = this.calculatorValue / 100
      }


      if (['X', '÷', '-', '+'].includes(n)) {
        if (n === '÷') {
          this.operator = '/'
        }
        else {
          this.operator = n
        }
        this.previousCalculatorValue = this.calculatorValue
        this.calculatorValue = '';
      }

      if (n === '=') {
        this.calculatorValue = eval(
          this.previousCalculatorValue + this.operator + this.calculatorValue
        );

        this.previousCalculatorValue = ''
        this.operator = null;
      }
    }
  }
}
</script>

<template>
  <div class="h-screen bg-vue-dark py-[50px]">

    <div class="w-full p-3 max-w-[400px] mx-auto bg-[#234] rounded">

      <!-- Calculator Result -->
      <div class="w-full rounded my-1 p-3 text-right font-weight-bold text-white bg-vue-dark">
        {{ calculatorValue || 0}}
      </div>

      <!-- Calculator buttons -->
      <div class="grid grid-cols-4 gap-[0.0005rem]">
        <div v-for="n in calculatorElements" :key="n">
          <div class="rounded text-white text-center m-1 py-4 bg-vue-dark hover-class"
            :class="{ 'bg-vue-green': ['C', 'X', '÷', '-', '+', '%', '='].includes(n) }" @click="action(n)">
            {{ n }}
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
.bg-vue-dark {
  background: #31475e;
}

.hover-class:hover {
  cursor: pointer;
  background: #3D5875;
}

.bg-vue-green {
  background: #3fb984;
}
</style>
