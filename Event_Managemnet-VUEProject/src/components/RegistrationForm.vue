<template>
  <section class="mb-5">
    <h2 class="text-center mb-4">Event Registration</h2>
    <form @submit.prevent="submitForm" class="needs-validation" novalidate>
      <div class="row">
        <div class="col-md-6 mb-3">
          <label for="username" class="form-label">Username</label>
          <input type="text" class="form-control" id="username" v-model="form.username" required>
          <div class="invalid-feedback">Please provide a username.</div>
        </div>
        
        <div class="col-md-6 mb-3">
          <label for="password" class="form-label">Password</label>
          <input type="password" class="form-control" id="password" v-model="form.password" required>
          <div class="invalid-feedback">Please provide a password.</div>
        </div>
        
        <div class="col-md-6 mb-3">
          <label for="confirmPassword" class="form-label">Confirm Password</label>
          <input type="password" class="form-control" id="confirmPassword" 
                 v-model="form.confirmPassword" 
                 @input="checkPasswordMatch"
                 required>
          <div class="invalid-feedback">Please confirm your password.</div>
          <div v-if="passwordMismatch" class="text-danger small">Passwords do not match!</div>
        </div>
        
        <div class="col-md-6 mb-3">
          <label class="form-label">Event Category</label>
          <div class="form-check" v-for="category in categories" :key="category">
            <input class="form-check-input" type="radio" :id="'category-' + category" 
                   v-model="form.selectedCategory" :value="category">
            <label class="form-check-label" :for="'category-' + category">
              {{ category }}
            </label>
          </div>
        </div>
        
        <div class="col-md-6 mb-3">
          <label for="eventName" class="form-label">Event Name</label>
          <select class="form-select" id="eventName" v-model="form.selectedEvent" required>
            <option value="" disabled>Select an event</option>
            <option v-for="event in filteredEventsByCategory" :key="event.id" :value="event.name">
              {{ event.name }}
            </option>
          </select>
          <div class="invalid-feedback">Please select an event.</div>
        </div>
      </div>
      
      <button type="submit" class="btn btn-primary" :disabled="passwordMismatch">Register</button>
    </form>
    
    <div v-if="submitted" class="mt-4 p-3 bg-light rounded">
      <h4>Registration Summary</h4>
      <p><strong>Username:</strong> {{ form.username }}</p>
      <p><strong>Selected Category:</strong> {{ form.selectedCategory }}</p>
      <p><strong>Selected Event:</strong> {{ form.selectedEvent }}</p>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    events: {
      type: Array,
      required: true
    },
    categories: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      form: {
        username: '',
        password: '',
        confirmPassword: '',
        selectedCategory: 'Business',
        selectedEvent: ''
      },
      passwordMismatch: false,
      submitted: false
    }
  },
  computed: {
    filteredEventsByCategory() {
      return this.events.filter(event => event.category === this.form.selectedCategory);
    }
  },
  methods: {
    checkPasswordMatch() {
      this.passwordMismatch = this.form.password !== this.form.confirmPassword;
    },
    submitForm() {
      this.submitted = true;
      this.$emit('form-submitted', this.form);
    }
  },
  watch: {
    'form.selectedCategory'() {
      this.form.selectedEvent = '';
    }
  }
}
</script>