<template>
  <div class="row">
    <div class="col">
      <h3>Arrange Payment Information</h3>
      <h5>Please Choose One</h5>
      <div
        class="payment-option"
        v-for="(paymentType, index) in selectedPlan.paymentTypes"
        :key="paymentType.type"
      >
        <Climb
          v-if="paymentType.type === 'climb'"
          :paymentType="paymentType"
          :number="index + 1"
          :plan="selectedPlan"
          css="paymentType ml-4"
        />
        <CreditCard
          v-if="paymentType.type === 'creditCard'"
          :paymentType="paymentType"
          :number="index + 1"
          css="paymentType ml-4"
        />
        <CallBack
          v-if="paymentType.type === 'callBack'"
          :number="index + 1"
          css="paymentType ml-4"
        />
      </div>
      <!-- <div v-if="selectedPlan.paymentTypes.financing">
          <hr class="mt-5 mb-5" />
          <h1>Or</h1>
          <h3>Financing</h3>
          <p>Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
      </div>-->

      <!-- <div v-if="selectedPlan.paymentTypes.isa">
          <hr class="mt-5 mb-5" />
          <h1>Or</h1>
          <h3>Income Sharing Agreement</h3>
          <p>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
      </div>-->

      <!-- <div v-if="selectedPlan.paymentTypes.callback">
          <hr class="mt-5 mb-5" />
          <h1>Or</h1>
          <h3>Continue With Admissions</h3>
          <p>In order to continue your enrollment, we need to speak with you. Please fill out the form below to set up a call with an Admissions Counselor to go over your options.</p>
          <HubspotForm formId="e7360a11-ca18-4b36-a73b-b8ccdce4f3e7" />
      </div>-->
    </div>
  </div>
</template>

<script>
import PlanCard from "@/components/PlanCard";
import HubspotForm from "@/components/HubspotForm";
import CreditCard from "@/components/paymentTypes/creditCard";
import CallBack from "@/components/paymentTypes/callback";
import Climb from "@/components/paymentTypes/climb";

export default {
  components: {
    PlanCard,
    HubspotForm,
    CreditCard,
    CallBack,
    Climb
  },
  data: () => ({
    selectedPlan: false
  }),
  created() {
    const applicant = this.$store.getters.getApplicant;
    if (!applicant) {
      this.$router.push("applicant");
    }
    this.selectedPlan = {
      ...this.$store.getters.getSelectedPlan,
      primary: true
    };
    this.$emit("changeStage", 3);
  }
};
</script>

<style lang="scss">
.payment-option {
  margin-top: 1.5rem;
  h3 {
    font-size: 1.1rem;
  }
}
</style>
