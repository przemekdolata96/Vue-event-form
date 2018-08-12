<template>
  <div class="form-part" id="when-form-part">
    <h4>When</h4>
    <div class="grid-container">
      <label :class="[{'error': startsIsIncorrect()}, 'requried']">Starts on</label>
      <div :class="[{'error': startsIsIncorrect()}, 'starts-on tooltip']">
        <input :class="[{'correct-input-border': !dateIsIncorrect()}]" required class="date-input" :min="getCurrentDate()" v-model="date" @change="correctDate()" v-on:input="$v.date.$touch" type="text" onfocus="(this.type='date')" onblur="(this.type='text')" name="date" placeholder="dd/mm/yyyy" aria-invalid="true">
        <label class="default-text-transform">at</label>
        <input :class="[{'correct-input-border': !timeIsIncorrect()}]" required id="time-input" v-model="time" v-on:input="$v.time.$touch" type="text" onfocus="(this.type='time')" onblur="(this.type='text')" placeholder="--:--" name="time" min="00:00" max="11:59">
        <label class="custom-radio-btn" for="am">
          <input v-model="timeOfDay" type="radio" name="time-of-day" id="am" value="AM" checked />
          <span class="custom-radio-outer">
            <span class="custom-radio-inner"></span>
          </span>
          AM
        </label>
        <label class="custom-radio-btn" for="pm">
          <input v-model="timeOfDay" type="radio" name="time-of-day" id="pm" value="PM" />
          <span class="custom-radio-outer">
            <span class="custom-radio-inner"></span>
          </span>
          PM
        </label>
        <span v-if="dateIsIncorrect()" class="tooltiptext">Date cannot be empty</span>
        <span v-if="timeIsIncorrect()" class="tooltiptext">Time cannot be empty</span>
      </div>
      <label>Duration</label>
      <div>
        <input v-model="duration" type="number" placeholder="Number" min="0" step="0.01">
        <label class="default-text-transform">hour</label>
      </div>
    </div>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate';
import { required } from 'vuelidate/lib/validators';

export default {
  name: 'FormWhen',

  data: () => {
    return {
      date: '',
      time: '',
      timeOfDay: 'AM',
      duration: '',
    }
  },

  mixins: [validationMixin],

  validations: {
    date: {
      required
    },
    time: {
      required
    },
  },

  methods: {
    dateIsIncorrect() {
      return this.$v.date.$error && this.$v.date.$dirty;
    },

    timeIsIncorrect() {
      return this.$v.time.$error && this.$v.time.$dirty;
    },

    startsIsIncorrect() {
      return this.dateIsIncorrect() || this.timeIsIncorrect();
    }, 

    getCurrentDate() {
      return new Date().toISOString().substring(0,10);
    },

    correctDate() {
      if(new Date > new Date(this.date)) {
        this.date = this.getCurrentDate();
      }
    }
    
  },

  updated() {
    this.$emit('when',{
      date: this.date,
      time: this.time,
      timeOfDay: this.timeOfDay,
      duration: this.duration,
    })
  }

}
</script>

<style scoped lang="scss">
  @import '../../theme/variables.scss';

  #when-form-part {

    label.default-text-transform {
      text-transform: none;
      color: $gray-500;
      font-size: 1rem;
    }

    .custom-radio-btn {
      color: $gray-500;
    }

    .starts-on {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
    }

    input[type="text"], input[type="date"] {
      width: 10rem !important;
      height: 2.5rem;
    }

    #time-input {
      width: 6rem !important;
      height: 2.5rem;
    }

    .correct-input-border {
      border-color: $gray;
    }
  }

  @media only screen and (max-width: 800px) {
    #when-form-part {

      input[type="text"], input[type="date"] {
        width: 100% !important;
      }

      #time-input {
        margin-top: 1rem;
      }
    }
  }
  
</style>
