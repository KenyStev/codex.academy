<template>
  <form @submit.prevent="confirmPlan">
    <h3>Confirm Program</h3>

    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Program:</th>
          <td>{{ selectedPlan.title }}</td>
        </tr>
        <tr>
          <th>Description:</th>
          <td>{{ selectedPlan.description }}</td>
        </tr>
        <tr>
          <th>Weekly Commitment:</th>
          <td>{{ selectedPlan.minimumWeeklyStudyHours }} hours</td>
        </tr>
        <!-- <tr>
          <th>Monthly:</th>
          <td>{{ dollars(selectedPlan.price) }}</td>
        </tr>
        <tr>
          <th>Total:</th>
          <td>{{ dollars(selectedPlan.total) }}</td>
        </tr>-->
        <tr v-if="selectedPlan.isBootcamp">
          <th>Program Duration:</th>
          <td>{{ selectedPlan.durationMonths }} months</td>
        </tr>
        <tr>
          <th>Start Date:</th>
          <td>
            <input type="date" v-model="startDate" :class="{error:!isValid(startDate)}" />
            <span
              class="alert alert-danger ml-3"
              role="alert"
              v-if="!isValid(startDate)"
            >Start date must be in the future.</span>
          </td>
        </tr>
      </table>
    </div>
    <p class="mt-4">
      <button class="btn btn-primary">
        <strong>Yes, Apply to This Program</strong>
      </button>
    </p>
    <p>
      <button class="btn btn-secondary" @click.prevent="changePlan">No, Change Program</button>
    </p>
  </form>
</template>

<script>
import Progress from "@/components/Progress";
import PlanSpread from "@/components/PlanSpread";
import EnrollForm from "@/components/EnrollForm";
import axios from "axios";
import { mapGetters } from "vuex";
import { required, minValue, maxValue } from "vuelidate/lib/validators";

var defaultDate = new Date();
defaultDate.setDate(defaultDate.getDate() + 7);

function getFutureDate() {
  var d = new Date();
  d.setDate(d.getDate() + 365);
  return d;
}

function getYesterday() {
  var d = new Date();
  d.setDate(d.getDate() - 1);
  return d;
}

export default {
  data() {
    return {
      selectedPlan: {},
      startDate: defaultDate.toISOString().split("T")[0]
    };
  },
  computed: {
    ...mapGetters(["getSelectedPlan", "getStartDate"])
  },
  components: {
    PlanSpread
  },
  methods: {
    isValid(startDate) {
      return new Date(startDate) > getYesterday();
    },
    dollars(num) {
      if (!num) return;
      return "$" + num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    confirmPlan() {
      if (this.isValid(this.startDate)) {
        this.$store.dispatch("setStartDate", this.startDate);
        this.$router.push("/enroll/applicant");
      }
    },
    changePlan() {
      this.$router.push("/findplan");
    }
  },
  mounted() {
    this.selectedPlan = this.$store.getters.getSelectedPlan;
    if (!this.selectedPlan) {
      this.$router.push("/findplan");
    }

    if (this.$store.getters.getStartDate) {
      this.startDate = this.$store.getters.getStartDate;
    }
  },
  created() {
    this.$emit("changeStage", 1);
  }
};
</script>

<style lang="scss" scoped>
@import "@/variables";
.error {
  border-color: red;
  background: #fdd;
}

.error:focus {
  outline-color: #f99;
}
</style>
</style>
