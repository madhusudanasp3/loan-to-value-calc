<template>
  <div>
    <div class="row">
      <div class="col">
        <div class="row">
          <div class="col">
            <div class="row padding-label font-weight-bold padding-left--text">
              <span>How To Use</span>
            </div>
            <div class="row padding-left--text">
              <p>The Loan-to-Value calculator is provided to help you estimate how much you owe on your mortgage compared to your home's current market value.</p>
              <p>This calculator returns information based on inputs regarding your existing mortgage information. It is important that you provide accurate information in order to receive more realistic results.</p>
            </div>
          </div>
        </div>
        <form id="form" @submit="submitForm">
          <div class="row">
            <div class="col">
              <div
                class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
              >
                <span>Existing Mortgage Information</span>
              </div>
            </div>
          </div>
          <InputFieldComponent
            v-bind:inputField="homeVal"
            v-on:enter-value="validateGreaterThanZero"
            v-bind:isInvalid="!homeVal.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageBalance.first"
            v-on:enter-value="validateGreaterThanZero"
            v-bind:isInvalid="!mortgageBalance.first.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageBalance.second"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageBalance.second.isValid"
          ></InputFieldComponent>
          <InputFieldComponent
            v-bind:inputField="mortgageBalance.third"
            v-on:enter-value="validateOptionalField"
            v-bind:isInvalid="!mortgageBalance.third.isValid"
          ></InputFieldComponent>

          <div class="form-group row">
            <div class="col">
              <div class="row">
                <button class="btn btn-secondary font-weight-bold">Calculate Results</button>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import InputFieldComponent from "./InputFieldComponent";
import { EventBus } from "./event-bus.js";

export default {
  name: "InputComponent",
  components: {
    InputFieldComponent
  },
  data() {
    return {
      homeVal: {
        id: "estHomeValue",
        value: null,
        isValid: true,
        label: "Estimated Home Value",
        icon: "$",
        type: "text",
        placeHolder: "Enter Amount",
        errorMsg:
          "This field is required and must be numeric and greater than zero."
      },
      mortgageBalance: {
        first: {
          id: "mortgageBalanceFirst",
          value: null,
          isValid: true,
          label: "First Mortgage Balance",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount",
          errorMsg:
            "This field is required and must be numeric and greater than Zero."
        },
        second: {
          id: "mortgageBalanceFirstSecond",
          value: null,
          isValid: true,
          label: "Second Mortgage Balance",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount Optional",
          errorMsg:
            "This field is optional, but if you enter it must be numeric or leave it blank.",
          desc: "(If applicable)"
        },
        third: {
          id: "mortgageBalanceFirstThird",
          value: null,
          isValid: true,
          label: "Third Mortgage Balance Or Any Other Liens On Your Home",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount Optional",
          errorMsg:
            "This field is optional, but if you enter it must be numeric or leave it blank.",
          desc: "(If applicable)"
        }
      },
      mortBalanceLTVRatio: {
        first: null,
        second: null,
        third: null,
        totalLTVRatio: "0.0",
        totalAmountOwed: null
      }
    };
  },
  methods: {
    validateOptionalField(input) {
      if (this.isFieldOptional(input.value)) {
        input.isValid = true;
      } else {
        input.isValid = this.isNumeric(input.value);
      }
    },
    validateGreaterThanZero(input) {
      input.isValid = this.isValid(input.value) && parseFloat(input.value) > 0;
    },
    isFieldOptional(n) {
      return n == null || this.isEmpty(n);
    },
    isValid(n) {
      // replace with regex
      return !this.isEmpty(n) && this.isNumeric(n);
    },
    isEmpty(n) {
      return n === "";
    },
    isNumeric(n) {
      // eslint-disable-next-line no-restricted-globals
      return !isNaN(parseFloat(n)) && isFinite(n);
    },
    submitForm(event) {
      if (this.isValidForm(this.homeVal, this.mortgageBalance)) {
        this.validateGreaterThanZero(this.homeVal);
        this.validateGreaterThanZero(this.mortgageBalance.first);
        event.preventDefault();
        return;
      }
      if (this.isFieldOptional(this.mortgageBalance.second.value)) {
        this.mortgageBalance.second.value = 0;
      }
      if (this.isFieldOptional(this.mortgageBalance.third.value)) {
        this.mortgageBalance.third.value = 0;
      }
      this.mortBalanceLTVRatio.totalAmountOwed = this.addNumberArr([
        this.mortgageBalance.first.value,
        this.mortgageBalance.second.value,
        this.mortgageBalance.third.value
      ]);
      this.mortBalanceLTVRatio.first = this.calculateRatio(
        this.mortgageBalance.first.value,
        this.homeVal.value
      );
      this.mortBalanceLTVRatio.second = this.calculateRatio(
        this.mortgageBalance.second.value,
        this.homeVal.value
      );
      this.mortBalanceLTVRatio.third = this.calculateRatio(
        this.mortgageBalance.third.value,
        this.homeVal.value
      );

      event.preventDefault();
      this.mortBalanceLTVRatio.totalLTVRatio = this.calculateRatio(
        this.mortBalanceLTVRatio.totalAmountOwed,
        this.homeVal.value
      );

      this.$emit(
        "submit-form",
        this.mortBalanceLTVRatio,
        this.mortgageBalance,
        this.homeVal.value
      );
      event.preventDefault();
    },
    calculateRatio(x, y) {
      return parseFloat(((x / y) * 100).toFixed(2));
    },
    addNumberArr(nums) {
      let sum = 0;
      for (let num of nums) {
        sum += parseFloat(num);
      }
      return sum;
    },

    isValidForm(homeVal, mortgageBalance) {
      let isFormValid = true;
      isFormValid =
        !homeVal.isValid ||
        !homeVal.value ||
        !mortgageBalance.first.value ||
        !mortgageBalance.first.isValid ||
        !mortgageBalance.second.isValid ||
        !mortgageBalance.third.isValid;
      return isFormValid;
    }
  },
  mounted() {
    EventBus.$on("caluclateRestls", event => {
      // eslint-disable-next-line
      console.log(
        `Your are inside inputcomponents block -- click value is ${event} clicks!`
      );
      this.submitForm(event);
    });
  }
};
</script>

<style lang="scss" scoped>
</style>