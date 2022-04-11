<template>
  <div>
    <div
      class="number"
      :id="'number-' + n"
      v-for="n in numbers"
      :key="n"
      @mouseover="setDivisors(n)"
      @mouseout="clearDivisors"
      ref="num"
      :class="{ active: divisors.includes(n) }"
    >
      {{ n }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      limit: this.$parent.limit,
      numbers: [],
      divisors: [],
    };
  },

  watch: {
    ["$parent.limit"](newLimit) {
      this.limit = newLimit;
      // this is so numbers are updated everytime input is changed
      this.setNumbers();
    },
  },

  beforeMount() {
    this.setNumbers();
  },

  methods: {
    setNumbers() {
      let numbers = [];
      for (var i = 0; i < this.limit; i++) {
        numbers = [...numbers, i];
      }
      const sortedNumbers = numbers.sort(() => Math.random() - 0.5);
      this.numbers = sortedNumbers;
    },

    setDivisors(number) {
      const nums = this.$refs.num;
      const array = [];
      for (let i = 0; i < nums.length; i++) {
        const num = nums[i].textContent.trim();
        if (number % num === 0) {
          array.push(Number(num));
        }
      }
      this.divisors = array;
    },

    clearDivisors() {
      this.divisors = [];
    },


    /* LEFT COMMENTS WHERE IMPROVEMENTS AND AMENDMENTS ARE REQUIRED,
    HAVE LEFT THIS UNCOMMENTED SO THAT COMMENTS ARE CLEAR TO READ,
    WILL NOT BE USING THE 3 FUNCTIONS BELOW */
    n() {
      let numbers = [];
      for (var i = 0; i < this.limit; i++) {
        numbers = [...numbers, i];
      }
      return numbers.sort(() => Math.random() - 0.5);
    },

    hov(number) {
      // we should not be accessing the dom in an imperative way, this is to be altered
      // using refs is a better way of accessing the number dom element
      const nums = document.querySelectorAll(".number");
      for (let i = 0; i < nums.length; i++) {
        const num = nums[i].textContent.trim();
        if (number % num === 0) {
          // should not be adding classes like this, vue allows for binding of classes in a declarative way
          nums[i].classList.add("active");
        }
      }
    },

    reset() {
      // use refs instead of imperatively accessing the DOM
      const nums = document.querySelectorAll(".number");
      // classes should not removed in an imperative way
      nums.forEach((num) => num.classList.remove("active"));
    },
  },
};
</script>

<style scoped>
.number {
  display: inline-block;
  padding: 5px;
  background-color: lightgrey;
  margin: 5px;
}
.active {
  background-color: red;
}
</style>
