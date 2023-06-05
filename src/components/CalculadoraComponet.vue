<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <table class="table table-bordered table-dark">
      <tbody>
        <tr>
          <td colspan="4" class="resultado">{{ output || 0 }}</td>
        </tr>
        <tr class="sobrecor">
          <td v-on:click="addNumber(7)">7</td>
          <td v-on:click="addNumber(8)">8</td>
          <td v-on:click="addNumber(9)">9</td>
          <td v-on:click="addOperate('/')">/</td>
        </tr>
        <tr class="sobrecor">
          <td v-on:click="addNumber(4)">4</td>
          <td v-on:click="addNumber(5)">5</td>
          <td v-on:click="addNumber(6)">6</td>
          <td v-on:click="addOperate('*')">*</td>
        </tr>
        <tr class="sobrecor">
          <td v-on:click="addNumber(1)">1</td>
          <td v-on:click="addNumber(2)">2</td>
          <td v-on:click="addNumber(3)">3</td>
          <td v-on:click="addOperate('-')">-</td>
        </tr>
        <tr class="sobrecor">
          <td v-on:click="addPonto('.')">.</td>
          <td v-on:click="addNumber(0)">0</td>
          <td v-on:click="porcent()">%</td>
          <td v-on:click="addOperate('+')">+</td>
        </tr>
        <tr class="sobrecor">
          <td v-on:click="clearField" class="clear">C</td>
          <td v-on:click="inverse()">+/-</td>
          <td colspan="1" class="del" v-on:click="deleteUltimoCaractere()">X</td>
          <td class="enter" v-on:click="calculate()">=</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Calculadora',
  props: {
    msg: String
  },
  data() {
    return {
      output: '0',
      previousValue: null,
      operator: null,
      accounts: []
    };
  },
  methods: {
    // Limpa o campo de saída
    clearField() {
      this.output = '0';
    },
    // Adiciona um número ao campo de saída
    addNumber(number) {
      if (this.output === '0') {
        this.output = number.toString();
      } else {
        this.output += number.toString();
      }
    },
    // Adiciona um operador ao campo de saída
    addOperate(operator) {
      if (this.output === '0') {
        this.output = operator.toString();
      } else {
        this.output += operator.toString();
      }
    },
    // Adiciona um ponto decimal ao campo de saída
    addPonto(value) {
      if (this.output === '0' && value !== '.') {
        this.output = value;
      } else {
        this.output += value;
      }
    },
    inverse(){
      // Verifica se a expressão contém um operador de inversão de sinal      
      const numericValue = parseFloat(this.output);
      const invertedValue = -numericValue;
      this.output = invertedValue.toString();
    },
    porcent(){
      // Verifica se a expressão contém um operador de porcentagem
      this.output = (parseFloat(this.output) / 100).toString();
      
    },
    // Realiza o cálculo com base na expressão no campo de saída
    calculate() {
      let expression = this.output;

      // Verifica se a expressão contém uma operação de soma
      if (expression.includes('+')) {
        const numbers = expression.split('+');
        const sum = numbers.reduce((acc, curr) => Number(acc) + Number(curr));
        this.output = sum.toString();
      }
      // Verifica se a expressão contém uma operação de subtração
      else if (expression.includes('-')) {
        const numbers = expression.split('-');
        const difference = numbers.reduce((acc, curr) => Number(acc) - Number(curr));
        this.output = difference.toString();
      }
      // Verifica se a expressão contém uma operação de multiplicação
      else if (expression.includes('*')) {
        const numbers = expression.split('*');
        const product = numbers.reduce((acc, curr) => Number(acc) * Number(curr));
        this.output = product.toString();
      }
      // Verifica se a expressão contém uma operação de divisão
      else if (expression.includes('/')) {
        const numbers = expression.split('/');
        const quotient = numbers.reduce((acc, curr) => Number(acc) / Number(curr));
        this.output = quotient.toString();
      }
      // Trata as operações de multiplicação e divisão primeiro
      if (expression.includes('*') || expression.includes('/')) {
        const multiplyDivideRegex = /(\d+\.?\d*)\s*([*/])\s*(\d+\.?\d*)/g;
        expression = expression.replace(multiplyDivideRegex, (match, num1, operator, num2) => {
          const n1 = Number(num1);
          const n2 = Number(num2);
          if (operator === '*') {
            return (n1 * n2).toString();
          } else if (operator === '/') {
            return (n1 / n2).toString();
          }
        });
      }

      // Trata as operações de soma e subtração
      if (expression.includes('+') || expression.includes('-')) {
          const addSubtractRegex = /(\d+\.?\d*)\s*([+\\-])\s*(\d+\.?\d*)/g;
          expression = expression.replace(addSubtractRegex, (match, num1, operator, num2) => {
          const n1 = Number(num1);
          const n2 = Number(num2);
          if (operator === '+') {
          return (n1 + n2).toString();
        } else if (operator === '-') {
          return (n1 - n2).toString();
          }
        });
      }
      this.output = expression;
      this.accounts.push(expression);
    },
    // Deleta o último caractere do campo de saída
    deleteUltimoCaractere() {
      this.output = this.output.substr(0, this.output.length - 1);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
td {
  user-select: none;
}
.enter{
  background-color: #1fc448;
}
.del{
  background-color: rgb(240, 18, 18);
}
.sobrecor{
  font-size: 40px;
}
.resultado{
  font-size: 40px;
}
.sobrecor :hover{
  background-color: rgb(163, 159, 159);
}
.clear{
  background-color: rgb(250, 175, 37);
}
.sobrecor :active{
  background-color: black;
}
h1{
  font-size: 60px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
