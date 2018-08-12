<template>
  <div class="form-part">
    <h4>About</h4>
    <div class="grid-container">
      <label :class="[{'error': $v.title.$error && $v.title.$dirty}, 'requried']">Title</label>
      <div :class="[{'error': $v.title.$error && $v.title.$dirty}, 'tooltip']">
        <input v-model.trim="$v.title.$model" class="input-full-width" type="text" placeholder="Make it short and clear" required>
        <span class="tooltiptext">Title cannot be empty</span>
      </div>
      <label id="description" :class="[{'error': $v.description.$error && $v.description.$dirty}, 'requried']">Description</label>
      <div :class="[{'error': $v.description.$error && $v.description.$dirty}, 'tooltip']">
        <textarea @input="descriptionCounter" v-on:input="$v.description.$touch" v-model="description" class="input-full-width" name="description" cols="8" rows="5" maxlength="140" placeholder="Write about your event, be creative" required></textarea>
        <span class="tooltiptext">Description requried</span>
        <div class="field-description">
          <span>Max length 140 characters</span>
          <span>{{descriptionCounter()}}/140</span>
        </div>
      </div>
      <label>Catergory</label>
      <div>
        <div @click="dropdownOpen" class="dropdown">
          <label v-if="category_id==''">Select Option</label>
          <label v-if="category_id!=''" class="category-label">{{categories[category_id].name}}</label>
          <ul id="category-dropdown" class="dropdown-options">
            <li  @click="optionChoose" data-category="" >-</li>
            <li :class="[{'active': category_id === category.id }]" v-for="(category) in categories" @click="optionChoose" :data-category="category.id" v-bind:key="category.id">{{category.name}}</li>
          </ul>
        </div>
        <div class="field-description">
          <span>Describies topic and people who should be interested in this event</span>
        </div>
      </div>
      <label>Payment</label>
      <div id="payment">
        <label class="custom-radio-btn" for="free-event">
          <input v-model="paid_event" type="radio" name="paid_event" id="free-event" value="false" checked />
          <span class="custom-radio-outer">
            <span class="custom-radio-inner"></span>
          </span>
          Free event
        </label>
        <label class="custom-radio-btn" for="paid-event">
          <input v-model="paid_event" type="radio" name="paid_event" id="paid-event" value="true" />
          <span class="custom-radio-outer">
            <span class="custom-radio-inner"></span>
          </span>
          Paid event
        </label>
        <input v-if="paid_event=='true'" v-model="event_fee" type="number" min="0.01" step="0.01" placeholder="Fee" required>
        <span v-if="paid_event=='true'" class="black">$</span>
      </div>
      <label>Reward</label>
      <div>
        <input v-model="reward" type="number" min="0" step="0.01" placeholder="Number">
        <span class="black">reward points for attendance</span>
      </div>
    </div>
  </div>
</template>

<script>
import categories from '../../mocks/categories.json';
import { validationMixin } from 'vuelidate';
import { required } from 'vuelidate/lib/validators';

export default {
  name: 'FormAbout',

  data: function () {
    return {
      title: '',
      description: '',
      category_id: '',
      paid_event: "false",
      event_fee: "",
      reward: '',
      categories: '',
    }
  },

  mixins: [validationMixin],

  validations: {
    title: {
      required
    },
    description: {
      required
    },
  },
  methods: {
    dropdownOpen() {
      const dropdown = document.getElementById("category-dropdown");
      dropdown.classList.add('dropdown-show');
    },

    optionChoose(event) {
      event.stopPropagation();
      event.preventDefault();
      const dropdown = document.getElementById("category-dropdown");
      dropdown.classList.remove('dropdown-show');
      this.category_id = parseInt(event.target.dataset.category);
    },

    fetchData() {
      this.categories = Array.from(categories);
    },

    descriptionCounter() {
      const description = document.getElementsByTagName('textarea')[0];
      if(description != undefined) {
        return description.value.length;
      } else {
        return 0;
      }
    },

  },

  created() {
    this.fetchData()
  },

  updated() {
    this.$emit('about',{
      title: this.title,
      description: this.description,
      category_id: this.category_id,
      paid_event: this.paid_event,
      event_fee: this.event_fee,
      reward: this.reward,
    })
  }
}
</script>

<style scoped lang="scss">
  @import '../../theme/variables.scss';

  #payment {
    .custom-radio-btn {
      color: $gray-500;
      text-transform: none;
      margin-right: 1rem;
      font-size: 0.9rem;
    }
  }

  .black {
    color: $gray-500 !important;
  }
  
  textarea {
    border: 1px solid $gray;
    border-color: $gray;
    outline: none;
    resize: none;
    padding: 0.5rem;
  }
  
  textarea::-webkit-input-placeholder {
    color: $gray !important;
  }

  textarea::-moz-placeholder {
    color: $gray !important;
  }
  
</style>
