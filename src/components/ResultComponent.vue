<template>
  <div>
    <div id="result" class="row margin--bottom">
      <div class="col">
        <div class="row padding-label font-weight-bold padding-left--text">
          <span class>Please Note:</span>
        </div>
        <div class="row padding-p padding-left--text">
          <p>These results have been calculated based on your inputs regarding your existing mortgage information. Your mortgage company may consider additional factors in determining your Loan-to-Value amount.</p>
          <p>Please save this information (as a PDF document) or email it to yourself, so that you may have it as a reference when you speak with your mortgage company or a housing counselor.</p>
          <i>Every situation is different. Contact your mortgage company or a housing counselor to determine your exact results.</i>
          <br />
          <br />
        </div>
        <div class="row">
          <div class="col">
            <div
              class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
            >
              <span>Your Loan-to-Value Information</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Estimated Home Value</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ '$ ' + formatNumber(result.estHomeValue) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >First Mortgage Balance</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.firstMortgageBalance) }}</span>
            </div>
          </div>
        </div>
        <div
          v-if="result.secondMortgageBalance > 0"
          class="form-group row background--gray background--rounded padding margin--bottom"
        >
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Second Mortgage Balance</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{'$ ' + formatNumber(result.secondMortgageBalance) }}</span>
            </div>
          </div>
        </div>
        <div
          v-if="result.thirdMortgageBalance > 0"
          class="form-group row background--gray background--rounded padding margin--bottom"
        >
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Third Mortgage Balance or Other Liens</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.thirdMortgageBalance) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Total Amount Owed On The Home</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.totalAmountOwedToHome) }}</span>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <div
              class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
            >
              <span class>Your Loan-to-Value (LTV) Ratio</span>
            </div>
          </div>
        </div>
        <div
          class="form-group row background--gray background--rounded padding margin--bottom align-items-center"
        >
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
            First Mortgage LTV Ratio
            <br />
            <span
              class="desc"
            >(This number may be used to determine your refinance and modification options.)</span>
          </label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ formatNumber(result.firstLTVRatio) + ' %'}}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Second Mortgage LTV Ratio</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ formatNumber(result.secondLTVRatio) + ' %' }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Third Mortgage LTV Ratio</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ formatNumber(result.thirdLTVRatio) + ' %'}}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Total Loan-to-Value Ratio</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ formatNumber(result.totalRatio) + ' %'}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResultComponent",
  props: {
    result: Object
  },
  methods: {
    formatNumber(num) {
      return Number(num).toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
    }
  }
};
</script>

<style lang="scss">
</style>