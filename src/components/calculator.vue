<template>
  <div class="calc">
    <h1>{{ msg }}</h1>
    <div class="userInterface">
      <input v-model="lableValue" class="table" />
      <div class="wrapler">
        <label
          ><input
            class="operands"
            type="radio"
            name="operand"
            value="operand1"
            v-model="operandSelected"
            checked
          />
          Операнд1</label
        >
        <label
          ><input
            class="operands"
            type="radio"
            name="operand"
            value="operand2"
            v-model="operandSelected"
          />
          Операнд2</label
        >
      </div>
      <label for="viewKeyboard"
        ><input
          type="checkbox"
          name=""
          id="viewKeyboard"
          v-model="viewKeyboard"
        />Скрыть клавиатуру</label
      >
      <div class="keyboard">
        <div class="numbers" v-show="!this.viewKeyboard">
          <input
            type="button"
            class="numbersButton"
            v-for="item in numbersButton"
            v-bind:key="item"
            v-bind:value="item"
            @click="setNumber(item)"
          />
        </div>
        <div class="operations">
          <button
            class="operationsButton"
            v-for="item in operationsButton"
            v-bind:key="item"
            @click="operation(item)"
          >
            {{ item }}
          </button>

          <button class="operationsButton" @click="sectorClear()">
            &lt;--
          </button>
          <button
            class="operationsButton"
            @click="
              () => {
                resetCalculator();
                this.lableValue = '';
              }
            "
          >
            reset
          </button>
          <button class="operationsButton" @click="result()">=</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "calculator",
  props: {
    msg: String,
  },
  data: () => ({
    operand1: 0,
    operand2: 0,
    operandSelected: "operand1",
    operationSelected: "",
    lableValue: "",
    viewKeyboard: false,
    numbersButton: ["7", "8", "9", "4", "5", "6", "1", "2", "3", "0", "."],
    operationsButton: ["+", "-", "/", "*", "^", "div"],
  }),
  methods: {
    resetCalculator: function () {
      this.operand1 = 0;
      this.operand2 = 0;
      this.operationSelected = "";
    },
    refreshValue: function () {
      this.lableValue = "";
      (this.operand1.length > 0
        ? (this.lableValue += this.operand1)
        : (this.lableValue += "")) +
        (this.operationSelected.length > 0
          ? (this.lableValue += this.operationSelected)
          : (this.lableValue += "")) +
        (this.operand2.length > 0
          ? (this.lableValue += this.operand2)
          : (this.lableValue += ""));
    },
    sectorClear: function () {
      //Hello from the CS1.6
      if (this.operandSelected === "operand1") {
        this.operand1 = this.operand1.substring(0, this.operand1.length - 1);
      } else {
        this.operand2 = this.operand2.substring(0, this.operand2.length - 1);
      }
      this.refreshValue();
    },

    setNumber: function (item) {
      if (this.operandSelected === "operand1") {
        this.operand1 += item;
      } else {
        this.operand2 += item;
      }
      this.refreshValue();
    },

    operation: function (item) {
      this.operationSelected = item;
      this.operandSelected = "operand2";

      this.refreshValue();
    },

    result: function () {
      if (this.operationSelected === "^") {
        this.lableValue = Math.pow(this.operand1, this.operand2);
      } else if (this.operationSelected === "div") {
        this.lableValue =
          (this.operand1 - (this.operand1 % this.operand2)) / this.operand2;
      } else {
        this.lableValue = eval(
          +this.operand1 + this.operationSelected + +this.operand2
        );
      }
      this.resetCalculator();
      this.operandSelected = "operand1";
      this.operand1 = this.lableValue;
    },
  },
  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.calc {
  margin: 0 calc(50% - 150px) 0;
}
.table {
  width: 100%;
  margin-bottom: 30px;
}
.keyboard,
.numbers {
  display: flex;
}
.numbers {
  flex-wrap: wrap;
  max-width: 90%;
  justify-content: space-evenly;
  align-content: space-between;
  &Button {
    flex-basis: 30%;
    min-height: 22%;
  }
}
.operations {
  max-width: 15px;
  &Button {
    width: 100%;
  }
}
button {
  display: flex;
  justify-content: center;
  align-content: center;
  align-items: center;
}
</style>
