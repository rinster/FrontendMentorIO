<template>
  <main>
    <div class="ageCalculator_container">
      <div class="ageCalc_inputs">
        <div class="ageCalc_inputsItem">
          <label v-bind:class="{ 'is-danger--text': dayError !== '' }">Day</label>
          <input name="day" placeholder="DD" :value="dayInput" v-on:change="updateData"
            v-bind:class="{ 'is-danger--border': dayError !== '' }" />
          <span v-if="dayError !== ''">{{ dayError }}</span>
        </div>
        <div class="ageCalc_inputsItem">
          <label v-bind:class="{ 'is-danger--text': monthError !== '' }">Month</label>
          <input name="month" placeholder="MM" :value="monthInput" v-on:change="updateData"
            v-bind:class="{ 'is-danger--border': monthError !== '' }" />
          <span v-if="monthError !== ''">{{ monthError }}</span>
        </div>
        <div class="ageCalc_inputsItem">
          <label v-bind:class="{ 'is-danger--text': yearError !== '' }">Year</label>
          <input name="year" placeholder="YYYY" :value="yearInput" v-on:change="updateData"
            v-bind:class="{ 'is-danger--border': yearError !== '' }" />
          <span v-if="yearError !== ''">{{ yearError }}</span>
        </div>
        <button @click="calcDate">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 46 44">
            <g fill="none" stroke="#FFF" stroke-width="2">
              <path d="M1 22.019C8.333 21.686 23 25.616 23 44M23 44V0M45 22.019C37.667 21.686 23 25.616 23 44" />
            </g>
          </svg>
        </button>
      </div>

      <div class="ageCalc_output">
        <h1>
          <span>{{ yearOutput }}</span> years
        </h1>
        <h1>
          <span>{{ monthOutput }}</span> months
        </h1>
        <h1>
          <span>{{ dayOutput }}</span> days
        </h1>
      </div>
    </div>
  </main>
</template>

<script>
import moment from "moment";
export default {
  name: "App",
  data() {
    return {
      dayInput: "",
      monthInput: "",
      yearInput: "",
      dayOutput: "--",
      monthOutput: "--",
      yearOutput: "--",
      errorsPresent: false,
      dayError: "",
      monthError: "",
      yearError: "",
    };
  },
  methods: {
    updateData: function (event) {
      if (this.errorsPresent) {
        this.clearErrors();
      }
      if (event.target.name === "day") {
        this.dayInput = event.target.value;
      } else if (event.target.name === "month") {
        this.monthInput = event.target.value;
      } else if (event.target.name === "year") {
        this.yearInput = event.target.value;
      }
    },
    calcDate: function () {
      this.verifyDate();

      if (!this.errorsPresent) {
        let start = moment(
          this.yearInput + "-" + this.monthInput + "-" + this.dayInput
        );
        let ends = moment();

        let data = moment.duration(ends.diff(start));
        let elapsed = data._data;
        this.yearOutput = elapsed.years;
        this.monthOutput = elapsed.months;
        this.dayOutput = elapsed.days;

        this.clearErrors();
      }
    },
    verifyDate: function () {
      let currentYear = new Date().getFullYear();
      // Day
      if (this.dayInput === "") {
        this.dayError = "This field is required";
        this.errorsPresent = true;
      } else if (
        isNaN(Number(this.dayInput)) ||
        Number(this.dayInput) < 1 ||
        Number(this.dayInput) > 31
      ) {
        this.dayError = "Must be a valid day";
        this.errorsPresent = true;
      }
      // Month
      if (this.monthInput === "") {
        this.monthError = "This field is required";
        this.errorsPresent = true;
      } else if (
        Number(this.monthInput) < 1 ||
        Number(this.monthInput) > 12 ||
        isNaN(Number(this.monthInput))
      ) {
        this.monthError = "Must be a valid month";
        this.errorsPresent = true;
      }
      // Year
      if (this.yearInput === "") {
        this.yearError = "This field is required";
        this.errorsPresent = true;
      } else if (isNaN(Number(this.yearInput))) {
        this.yearError = "Must be a valid year";
        this.errorsPresent = true;
      } else if (Number(this.yearInput) > currentYear) {
        this.yearError = "Must be in the past";
        this.errorsPresent = true;
      }

      // Check entire DateString, ie(31-04-1991) April has only 30 days
      let dateString =
        this.yearInput + "-" + this.monthInput + "-" + this.dayInput;
      let checkMoment = moment(dateString);
      let isValidDate = checkMoment.isValid();
      if (
        !isValidDate &&
        this.yearInput !== "" &&
        this.dayInput !== "" &&
        this.monthInput !== ""
      ) {
        this.errorsPresent = true;
        this.dayError = "Invalid Date";
        this.monthError = "Invalid Date";
        this.yearError = "Invalid Date";
      }
    },
    clearErrors: function () {
      // reset errors and clear the danger
      this.errorsPresent = "";
      this.dayError = "";
      this.monthError = "";
      this.yearError = "";
    },
  },
};
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: hsl(0, 0%, 86%);
  font-family: "Poppins", sans-serif;
}

main {
  display: flex;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
}

.ageCalculator_container {
  background-color: hsl(0, 0%, 100%);
  padding: 3rem;
  border-radius: 20px 20px 30% 20px;
}

.ageCalc_inputs {
  display: flex;
  border-bottom: 1px solid hsl(0, 0%, 86%);
  padding-bottom: 40px;
  position: relative;
  width: 550px;
}

.ageCalc_inputsItem {
  display: flex;
  flex-direction: column;
  margin-right: 15px;
}

.ageCalc_inputsItem span {
  font-size: 11px;
  color: hsl(0, 100%, 67%);
  font-weight: 400;
  font-style: italic;
}

h1 {
  font-size: 70px;
  color: hsl(0, 0%, 8%);
  font-style: italic;
  margin-bottom: -25px;
}

.ageCalc_output span {
  color: hsl(259, 100%, 65%);
  font-size: 1.1em;
}

label {
  color: hsl(0, 1%, 44%);
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 0.125em;
  margin-bottom: 5px;
}

input {
  font-size: 32px;
  padding: 11px;
  width: 120px;
  font-weight: 700;
  border-radius: 8px;
  border: 1px solid hsl(0, 0%, 86%);
  margin-bottom: 5px;
}

input::placeholder {
  color: rgb(192, 192, 192);
  font-size: 0.9em;
}

button {
  background-color: hsl(259, 100%, 65%);
  border: 1px solid hsl(259, 100%, 65%);
  border-radius: 50%;
  height: 85px;
  width: 85px;

  position: absolute;
  bottom: -48px;
  right: -20px;
}

svg {
  height: 50px;
  width: 50px;
}

.is-danger--border {
  border: 1px solid hsl(0, 100%, 67%);
}

.is-danger--text {
  color: hsl(0, 100%, 67%);
}

@media screen and (max-width: 640px) {
  h1 {
    font-size: 60px;
  }

  .ageCalculator_container {
    padding: 2.7rem;
  }

  .ageCalc_inputs {
    width: 100%;
    margin-bottom: 30px;
    padding-bottom: 50px;
  }

  input {
    font-size: 22px;
    width: 110px;
  }

  button {
    left: 38%;
    height: 60px;
    width: 60px;
    bottom: -27%;
  }

  svg {
    height: 30px;
    width: 30px;
  }
}

@media screen and (max-width: 375px) {
  input {
    font-size: 22px;
    width: 80px;
  }

  h1 {
    font-size: 50px;
  }

  .ageCalculator_container {
    padding: 2.2rem;
  }

  button {
    left: 36%;
  }
}
</style>
