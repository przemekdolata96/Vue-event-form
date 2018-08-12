<template>
  <div id="app">
    <header>
      <div>
        <h2>New event</h2>
      </div>
    </header>
    <form action="location.href" v-on:submit.prevent="createEvent" v-if="!eventCreated">
      <div class="form-center">
        <FormAbout @about="onAbout"></FormAbout>
      </div>
      <div class="form-center">
        <FormCoordinator @coordinator="onCoordinator"></FormCoordinator>
      </div>
      <div class="form-center">
        <FormWhen @when="onWhen"></FormWhen>
      </div>
      <div id="submit">
        <button type="submit" >Publish event</button>
      </div>
    </form>
    <FormSummary v-if="eventCreated"></FormSummary>
  </div>
</template>

<script>
import FormAbout from './components/form/FormAbout.vue';
import FormCoordinator from './components/form/FormCoordinator.vue';
import FormWhen from './components/form/FormWhen.vue';
import FormSummary from './components/form/FormSummary.vue';

export default {
  name: 'app',

  components: {
    FormAbout,
    FormCoordinator,
    FormWhen,
    FormSummary
  },

  data: function () {
    return {
      title: null,
      description: null,
      category_id: null,
      paid_event: null,
      event_fee: 0,
      reward: 0,
      id: null,
      email: null,
      date: null,
      time: null,
      timeOfDay: null,
      duration: null,
      aboutCorrect: false,
      coordinatorCorrect: true,
      whenCorrect: false,
      eventCreated: false,
    }
  },

  methods: {

    onAbout(payload) {
      this.title = payload.title;
      this.description = payload.description;
      this.category_id = payload.category_id;
      this.paid_event = payload.paid_event,
      this.event_fee = payload.event_fee;
      this.reward = payload.reward;
      this.aboutCorrect = payload.aboutCorrect;
    },

    onCoordinator(payload) {
      this.id = payload.id;
      this.email = payload.email;
      this.coordinatorCorrect = payload.coordinatorCorrect;
    },

    onWhen(payload) {
      this.date = payload.date
      this.time = payload.time
      this.timeOfDay = payload.timeOfDay
      this.duration = payload.duration
      this.whenCorrect = payload.whenCorrect;
    },

    createEvent() {
      this.eventCreated = true;

      const HOUR_IN_SECONDS = 3600;
      const durationInSeconds = this.duration * HOUR_IN_SECONDS;

      if(this.paid_event === "false") {
        this.event_fee = 0;
      }

      if(this.reward === "") {
        this.reward = 0;
      }

      if(this.category_id !== "" && this.category_id !== null) {
        this.category_id = parseInt(this.category_id);
      } else {
        this.category_id = null;
      }

      let time = (() => { 
        if(this.timeOfDay === 'AM') {
          return this.time
        } else {
          let hours = parseInt(this.time.substring(0,2));
          hours += 12;
          let minutes = this.time.substring(3);
          return `${hours}:${minutes}`;
        }
      })();

      let date = `${this.date}T${time}`;

      const json = {
        title: this.title,
        description: this.description,
        category_id: this.category_id,
        paid_event: (this.paid_event === "true"),
        event_fee: parseFloat(this.event_fee),
        reward: parseFloat(this.reward),
        date: date,
        duration: durationInSeconds,
        coordinator: {
          email: this.email,
          id: this.id + '',
        },
      }
      console.log(json);
    }
  },
}
</script>

<style lang="scss">

@import './theme/variables.scss';
@import url('https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700');

* {
  margin: 0;
  font-family: 'Roboto', Helvetica, Arial, sans-serif;
  font-size: 16px;
  box-sizing: border-box;
  color: $gray-500;
}
#app {
  background-color: $gray-100;
  min-height: 100vh;
  padding-bottom: 1rem;
  width: 100%;
  color: $gray-500;

  header { 
    background-color: $indigo-dark;
    height: 100px;
    position: relative;

    div {
      background-color: $indigo;
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 80px;
      display: flex;
      justify-content: center;

      h2 {
        color: $white;
        font-weight: 100;
        font-size: 2rem;
        bottom: 0;
        display: block;
        width: 800px;
        height: 80px;
        padding-left: 1rem;
        line-height: 80px;
      }
    }
  }

  button {
    margin: 2rem;
    padding: 1rem;
    color: $white;
    background-color: $orange;
    border: none;
    text-transform: uppercase;
    border-radius: 0.1rem;
    cursor: pointer;
  }

  select, input {
    border: solid 1px $gray;
    border-radius: 0.2rem;
    padding-left: 1rem;

    &:required:invalid {
      color: $gray;
    }

    &::placeholder {
      color: $gray;
    }
  }

  .dropdown {
    position: relative;
    width: 100%;
    border: solid 1px $gray;
    border-radius: 0.2rem;
    padding: 0.5rem;
    display: flex;
    justify-content: left;
    align-items: center;
    
    label {
      color: $gray-500;
      text-transform: none !important;
      font-size: 1rem;
      height: 1rem;
      margin: 0;
    }

    .dropdown-show {
      visibility: visible !important;
    }

    .dropdown-options {
      visibility: hidden;
      display: block;
      position: absolute;
      top:-1rem;
      list-style-type: none;
      background-color: #F0F0F0;
      width: 100%;
      border-radius: 5px;
      padding: 1rem 0;
      z-index: 1000;
      box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.01);
      transition: all 0.3s cubic-bezier(.25,.8,.25,1);

      span {
        padding-left: 1rem;
        color: $gray-300;
      }

      li {
        color: $black;
        width: 100%;
        padding: 0.2rem;
        padding-left: 2rem;
        font-size: 0.9rem;

        &:hover {
          color: white;
          background-color: #4A9BFE;
          cursor: pointer;
        }
      }

      .active {
        
        &:before {
          position: absolute;
          left: 5px;
          content: '✓';
          font-weight: bold;
          color: $black;
        }
      }
    }

    &:after {
      position: absolute;
      right: 1rem;
      top: 50%;
      content: '';
      height: 0;
      border-left: 7px solid transparent;
      border-right: 7px solid transparent;
      border-top: 7px solid #e8e8e8;
    }
  }
  
  
  input {
    padding: 0.5rem;
  }

  .input-full-width {
    width: 100%;
  }

  .form-center {
    width: 100%;
    display: flex;
    justify-content: space-around;
  }

  .tooltip {
    position: relative;
    display: inline;
  }

  .tooltip .tooltiptext {
    position: absolute;
    width:160px;
    font-size: 0.8rem;
    color: #FFFFFF;
    background: $red-300;
    min-height: 30px;
    padding: 0.5rem;
    text-align: center;
    visibility: hidden;
    border-radius: 1px;
  }

  .tooltip .tooltiptext::after {
    content: '';
    position: absolute;
    top: 50%;
    right: 100%;
    margin-top: -8px;
    width: 0; height: 0;
    border-right: 8px solid $red-300;
    border-top: 8px solid transparent;
    border-bottom: 8px solid transparent;
  }

  .error {
    visibility: visible;
    color: $red-100;

    input, textarea, .custom-select {
      border-color: $red-100;
    }

    .tooltiptext {
      visibility: visible;
      opacity: 0.8;
      left: 100%;
      top: 0;
      margin-left: 15px;
      z-index: 999;
    }
  }

  input[type="number"] {
    appearance: none;
    -webkit-appearance: none;
    -moz-appearance: textfield;
    width: 6rem;
    margin-right: 1rem;
  }
  
  label {
    font-size: 17px;
    display: inline-block;
    margin-top: 0.5rem;
    color: $indigo-200;
    text-transform: uppercase;
    font-size: 0.8rem;
  }

  .requried {

    &::after {
      content: "*";
      color: $red-100;
      margin-left: 0.2rem;
    }
  }

  form {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    input[type="radio"] {
      display: none;
    }

    .custom-radio-outer {
      width: 15px;
      height: 15px;
      display: inline-block;
      border: 1px solid  $indigo-200;
      border-radius: 50%;
      vertical-align: text-bottom;
      margin-right: 5px;
      position: relative;
    }

    .custom-radio-inner {
      transform: scale(0);
      -webkit-transform: scale(0);
      -moz-transform: scale(0);
      transition: all 200ms ease-in-out;
      opacity: 0;
      background: $indigo-200;
      width: 9px;
      height: 9px;
      border-radius: 50%;
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      margin: auto;
      border: 1px solid $indigo-200;
    }

    input[type="radio"]:checked + .custom-radio-outer .custom-radio-inner {
      -webkit-transform: scale(1);
      -moz-transform: scale(1);
      transform: scale(1);
      opacity: 1;
    }

    #payment {
      display: flex;
      align-items: center;
      min-height: 3rem;
    }

    .form-part {
      padding: 1.5rem;
      margin: 2rem  3rem 0 3rem;
      width: 800px;
      background-color: $white;
      box-shadow: 0 1px 9px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.01);
      transition: all 0.3s cubic-bezier(.25,.8,.25,1);
      
      h4 {
        color: $indigo;
        border-bottom: 1px solid $gray;
        padding-bottom: 1rem;
        font-size: 1.2rem;
        font-weight: 400;
      }

      .grid-container {
        margin-top: 2rem;
        display: grid;
        grid-gap: 1rem;
        grid-template-columns: 15% 60%;
        align-items: top;
      }

      .field-description {
        display: flex;
        justify-content: space-between;

        span {
          color: $gray;
          font-size: 0.7rem;
          font-style: italic;
        }
      }
    }
  }
}

@media only screen and (max-width: 800px) {

  .tooltip .tooltiptext {
    left: 30% !important;
    top: -2.5rem !important;
    padding: 2rem;
  }

  .tooltiptext {
    height: 1rem;
  }

  span.black {
    font-size: 0.8rem;
  }

  .starts-on {
    flex-wrap: wrap;
  }

  #app {
    overflow: hidden;

    form {

      .form-part {
        margin: 0.5rem;
      }

      .grid-container {
        grid-template-columns: 100% !important;
      }

      #payment {
        flex-wrap: wrap;
        align-items: start !important;
        height: 4rem;

        label {
          margin-bottom: 1rem;
        }

        span.black {
          align-self: center;
        }
      }
    }
  }
}
</style>
