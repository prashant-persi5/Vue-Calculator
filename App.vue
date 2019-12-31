<template lang="html">
  <main class="calc">
    <div class="display">
      <div class="mem">{{calcStore}}</div>
      <div class="disp">{{display}}</div>
    </div>
    <div class="buttons">
      <div
        class="button-row"
        v-for="row in buttonRows"
        :key="row[0].text">
        <div
          @click="buttonClicks[button.type](button)"
          class="button"
          v-for="button in row"
          :key="button.text">
          {{button.text}}
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  methods: {
    performOperation() {
      this.display = this.operations[this.currentOperator](+this.prevValue, +this.display);
    }
  },
  data: (vm) => ({
    display: 0,
    currVal: 0,
    calcStore: '\xa0',
    prevValue: '',
    secondVal: 0,
    currentOperator: '',
    buttonClicks: {
      number(button) {
        if (button.text == '.' && vm.display.toString().indexOf('.') !== -1) return;
        if (vm.display == 0 || vm.secondVal == 1) {
          vm.display = button.text;
          vm.secondVal = 0;
        } else {
          vm.display += button.text;
        }
        vm.currVal = vm.display;
      },
      operator(button) {
        if(vm.currentOperator) {
          vm.performOperation();
        }
        vm.currentOperator = button.text;
        vm.calcStore += ' ' + vm.currVal + ' ' + vm.currentOperator;
        vm.prevValue = Number(vm.display);
        vm.secondVal = 1;
      },
      special(button) {
        if (button.text == 'CE') {
          vm.display = 0;
        } else if (button.text == 'C') {
          vm.display = 0;
          vm.calcStore = '\xa0';
          vm.prevValue = '';
          vm.currentOperator = '';
          vm.secondVal = 1;
        } else if (button.text == '<') {
          vm.display = vm.display.substr(0, vm.display.length - 1);
          vm.display = (vm.display) ? vm.display : 0;
        } else if (button.text == '+/-') {
          vm.display *= -1; 
        } else if (button.text == '=') {
          vm.performOperation();
          vm.calcStore = '\xa0';
          vm.prevValue = '';
          vm.currentOperator = '';
          vm.secondVal = 1;
        }
      },
    },
    operations: {
      '+' : (a, b) => a + b,
      '-' : (a, b) => a - b,
      'x' : (a, b) => a * b,
      '/' : (a, b) => a / b,
    },
    buttonRows: [
      [
        { text: 'CE', type: 'special' },
        { text: 'C', type: 'special' },
        { text: '<', type: 'special' },
        { text: '/', type: 'operator' }
      ],
      [
        { text: '7', type: 'number' },
        { text: '8', type: 'number' },
        { text: '9', type: 'number' },
        { text: 'x', type: 'operator' }
      ],
      [
        { text: '4', type: 'number' },
        { text: '5', type: 'number' },
        { text: '6', type: 'number' },
        { text: '-', type: 'operator' }
      ],
      [
        { text: '1', type: 'number' },
        { text: '2', type: 'number' },
        { text: '3', type: 'number' },
        { text: '+', type: 'operator' }
      ],
      [
        { text: '+/-', type: 'special' },
        { text: '0', type: 'number' },
        { text: '.', type: 'number' },
        { text: '=', type: 'special' }
      ],
    ]
  })
};
</script>

<style>
body {
  width: 95vw;
  height: 95vh;
  font-size: 3vw;
  display: flex;
  justify-content: center;
  align-items: center;
}
.calc {
  width: 50vw;
  height: 70vh;
  font-family: sans-serif;
  border-radius: 5px;
  overflow: hidden;
  border: 0.5px solid grey;
}
.mem {
  font-size: 2.5vw;
}
.display {
  border: 1px solid grey;
  text-align: right;
  background: #989898;
  color: #fff;
  height: 10vh;
}
.buttons {
  text-align: center;
  font-weight: 600;
  height: 60vh;
}
.button-row {
  width: 100%;
  height: 20%;
  display: flex;
  justify-content: space-around; 
}
.button {
  display: inline-block;
  outline: 1px solid grey;
  width: 100%;
  background: #D0D0D0;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-basis: calc(100% / 2);
}
.button-row .button:last-child {
  background: #F6872C;
  color: #fff;
}
.button-row .button:active, .button-row .button:hover {
  background: #b5b4b3; 
}
</style>