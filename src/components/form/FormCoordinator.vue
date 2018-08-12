<template>
 <div class="form-part">
   <h4>Coordinator</h4>
   <div class="grid-container">
     <label :class="[{'error': $v.id.$error && $v.id.$dirty}, 'requried']">Responsible</label>
     <div :class="[{'error': $v.id.$error && $v.id.$dirty}, 'tooltip']">
        <div @click="dropdownOpen" class="dropdown">
          <label class="coordinator-label">
            <span v-if="id==user_id">Me - {{employes[id].name}} {{employes[id].lastname}}</span>
            <span v-if="id!=user_id">Others - {{employes[id].name}} {{employes[id].lastname}}</span>
          </label>
          <ul id="coordinator-dropdown" class="dropdown-options">
            <span>Me</span>
              <li :class="[{'active': id == user.id }]" @click="optionChoose" :data-id="user.id">{{user.name}} {{user.lastname}}</li>
            <span>Others</span>
              <li :class="[{'active': id == other.id }]" v-for="(other) in others" @click="optionChoose" :data-id="other.id" v-bind:key="other.id">{{other.name}} {{other.lastname}}</li>
          </ul>
        </div>
        <span class="tooltiptext">Responsible cannot be empty</span>
      </div>
     <label :class="[{'error': $v.email.$invalid}]" >Email</label>
    <div :class="[{'error': $v.email.$invalid}, 'tooltip']">
      <input v-model="email" class="input-full-width" type="email" name="email" placeholder="Email">
      <span class="tooltiptext">Email must be correct</span>
    </div>
   </div>
 </div>
</template>

<script>
import employes from '../../mocks/employes.json';
import { validationMixin } from 'vuelidate';
import { required, email } from 'vuelidate/lib/validators';

export default {
  name: 'FormCoordinator',

  data: () => {
    return {
      user_id:3,
      id: '',
      email: '',
      employes: '',
      others: '',
      user: '',
    }
  },

  mixins: [validationMixin],

  validations: {
    id: {
      required
    },
    email: {
      email
    }
  },

  methods: {

    dropdownOpen() {
      const dropdown = document.getElementById("coordinator-dropdown");
      dropdown.classList.add('dropdown-show');
    },

    optionChoose(event) {
      event.stopPropagation();
      event.preventDefault();
      const dropdown = document.getElementById("coordinator-dropdown");
      dropdown.classList.remove('dropdown-show');
      this.id = event.target.dataset.id;
      this.email = employes[event.target.dataset.id].email;
    },

    fetchData() {
      this.employes = Array.from(employes);
      this.others = [].concat(Array.from(employes).splice(0,this.user_id),Array.from(employes).splice(this.user_id+1));
      this.user = Array.from(employes)[this.user_id];
      this.id = 3;
      this.email = employes[this.user_id].email;
    }

  },

  created() {
    this.fetchData()
    this.$emit('coordinator',{
      id: this.id,
      email: this.email,
    })
  },

  updated() {
    this.$emit('coordinator',{
      id: this.id,
      email: this.email,
    })
  }
}
</script>

<style scoped lang="scss">

</style>
