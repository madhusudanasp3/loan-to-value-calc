<template>
  <div class="calculator">
    <div class="container">
      <div class="row">
        <div class="col text-left page-header padding-left--text margin--bottom">
          <h4>{{ msg }}</h4>
        </div>
      </div>
      <nav>
        <div id="nav-tab" class="nav nav-tabs" role="tablist">
          <a
            id="input-tab"
            class="nav-item nav-link"
            data-toggle="tab"
            href="#input-tab-content"
            role="tab"
            aria-controls="input"
            aria-selected="true"
            :class="{ 'active': visibleTab === 0}"
            @click="visibleTab = 0"
          >Input</a>
          <a
            id="result-tab"
            class="nav-item nav-link"
            data-toggle="tab"
            href="#result-tab-content"
            role="tab"
            aria-controls="result"
            aria-selected="false"
            :class="[result.totalRatio == null ? 'disabled1': '',
                     visibleTab === 1 ? 'active' : '']"
            @click="resultTabbehaviour"
          >Result</a>
        </div>
      </nav>
      <div id="tabContent" class="tab-content">
        <div
          id="input-tab-content"
          class="tab-pane fade"
          role="tabpanel"
          aria-labelledby="input-tab"
          :class="{ 'show active': visibleTab === 0 }"
        >
          <InputComponent v-on:submit-form="displayResults"></InputComponent>
        </div>
        <div
          id="result-tab-content"
          class="tab-pane fade"
          role="tabpanel"
          aria-labelledby="result-tab"
          :class="{ 'show active': visibleTab === 1 }"
        >
          <ResultComponent v-bind:result="result"></ResultComponent>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import InputComponent from "./InputComponent.vue";
import ResultComponent from "./ResultComponent.vue";
import { EventBus } from "./event-bus.js";
export default {
  name: "LoanToValueCalc",
  components: {
    InputComponent,
    ResultComponent
  },
  data() {
    return {
      msg: "Loan-to-Value Calculator",
      result: {
        estHomeValue: null,
        firstMortgageBalance: null,
        secondMortgageBalance: null,
        thirdMortgageBalance: null,
        firstLTVRatio: null,
        secondLTVRatio: null,
        thirdLTVRatio: null,
        totalRatio: null,
        totalAmountOwedToHome: null
      },
      visibleTab: 0,
      resultTabClick: 0
    };
  },

  methods: {
    displayResults(calculatedResults, estMortgages, estimatedHomeVal) {
      this.result.firstLTVRatio = parseFloat(calculatedResults.first);
      this.result.secondLTVRatio = parseFloat(calculatedResults.second);
      this.result.thirdLTVRatio = parseFloat(calculatedResults.third);
      this.result.totalRatio = parseFloat(calculatedResults.totalLTVRatio);
      this.result.totalAmountOwedToHome = parseFloat(
        calculatedResults.totalAmountOwed
      );
      this.result.estHomeValue = parseFloat(estimatedHomeVal);
      this.result.firstMortgageBalance = parseFloat(estMortgages.first.value);
      this.result.secondMortgageBalance = parseFloat(estMortgages.second.value);
      this.result.thirdMortgageBalance = parseFloat(estMortgages.third.value);
      this.visibleTab = 1;
    },
    resultTabbehaviour(event) {
      this.resultTabClick = 1;
      if (
        this.result.estHomeValue == null ||
        this.firstMortgageBalance == null
      ) {
        event.preventDefault();
        event.stopPropagation();
      }
      EventBus.$emit("caluclateRestls", event);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
</style>
