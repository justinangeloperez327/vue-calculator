<script setup>
import { ref } from 'vue';

const current = ref(0);
const maxLength = 15;

const append = (char) => {
  if(current.value === 0) {
    current.value = '';
  }
  if (current.value.length < maxLength) {
    current.value += char;
  }
};

const clear = () => {
  current.value = 0;
};

const deleteLast = () => {
  current.value = current.value.slice(0, -1);
};

const calculate = () => {
  try {
    let result = safeEval(current.value).toString();
    if (result.length > maxLength) {
      result = result.slice(0, maxLength);
    }
    current.value = result;
  } catch (e) {
    current.value = 'Error';
  }
};

const safeEval = (expression) => {
  // Replace the operators with spaces to split the expression into tokens
  const tokens = expression.split(/([+\-*/])/).filter(Boolean);
  let stack = [];
  let operator = null;

  tokens.forEach(token => {
    if (['+', '-', '*', '/'].includes(token)) {
      operator = token;
    } else {
      let num = parseFloat(token);
      if (operator && stack.length) {
        let prevNum = stack.pop();
        switch (operator) {
          case '+':
            num = prevNum + num;
            break;
          case '-':
            num = prevNum - num;
            break;
          case '*':
            num = prevNum * num;
            break;
          case '/':
            num = prevNum / num;
            break;
        }
        operator = null;
      }
      stack.push(num);
    }
  });

  return stack[0];
};

</script>

<template>
  <div class="calculator">
    <div class="display">{{ current }}</div>
    <hr class="border-line">
    <div class="buttons">
      <button @click="clear" class="button-ac">AC</button>
      <button @click="deleteLast" class="button-c">C</button>
      <button @click="append('%')" class="button-percentage">%</button>
      <button @click="append('/')" class="button-operator">/</button>

      <button @click="append('7')" class="button-number">7</button>
      <button @click="append('8')" class="button-number">8</button>
      <button @click="append('9')" class="button-number">9</button>
      <button @click="append('*')" class="button-operator">x</button>

      <button @click="append('4')" class="button-number">4</button>
      <button @click="append('5')" class="button-number">5</button>
      <button @click="append('6')" class="button-number">6</button>
      <button @click="append('-')" class="button-operator">-</button>

      <button @click="append('1')" class="button-number">1</button>
      <button @click="append('2')" class="button-number">2</button>
      <button @click="append('3')" class="button-number">3</button>
      <button @click="append('+')" class="button-operator">+</button>

      <button @click="append('.')" class="button-number">.</button>
      <button @click="append('0')" class="button-number zero">0</button>
      <button @click="calculate()" class="button-equals">=</button>
    </div>
  </div>
</template>

<style scoped>
.calculator {
  display: flex;
  flex-direction: column;
  width: 400px;
  margin: auto;
  border: 2px solid #333;
  border-radius: 1rem;
  box-shadow: 10px 10px 20px rgb(0, 0, 0);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #000000;
}
.display {
  background-color: #727272;
  color: black;
  font-size: 2rem;
  padding: .5rem;
  margin-block: 1.8rem;
  margin-inline: 3rem;
  text-align: right;
  height: 60px; /* Added fixed height */
}
.border-line {
  border: 2px solid #727272;
  width: 100%;
}

.buttons {
  margin-block: 1.5rem;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2rem;
  margin-inline: 2rem;
}
button {
  font-size: 1.5rem;
  padding: .5rem;
  background-color: #444;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:active {
  transform: scale(0.98);
}
.button-operator {
  background-color: #00a79d;
}
.button-equals {
  background-color: #ef2938;
}
.button-percentage {
  background-color: #ef2938;
}
.button-ac {
  background-color: #ef2938;
}
.button-c {
  background-color: #ef2938;
}
.zero {
  grid-column: span 2;
}
</style>
