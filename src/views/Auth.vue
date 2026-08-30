<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-6 offset-md-3">
        <div class="card shadow">
          <div class="card-header bg-success text-white">
            <h3 class="card-title mb-0">Login or Register</h3>
          </div>
          <div class="card-body">
            <div class="alert alert-info mb-4">
              <strong>Demo Login:</strong> Any valid details will "log you in" to see the Resources page.
            </div>
            <form @submit.prevent="submitForm">
              
              <!-- Username Field -->
              <div class="mb-3">
                <label for="username" class="form-label">Username</label>
                <input 
                  type="text" 
                  class="form-control" 
                  id="username" 
                  v-model="formData.username"
                  @blur="() => validateUsername(true)"
                  @input="() => validateUsername(false)"
                >
                <div v-if="errors.username" class="text-danger mt-1">
                  {{ errors.username }}
                </div>
              </div>

              <!-- Email Field -->
              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input 
                  type="email" 
                  class="form-control" 
                  id="email" 
                  v-model="formData.email"
                  @blur="() => validateEmail(true)"
                  @input="() => validateEmail(false)"
                >
                <div v-if="errors.email" class="text-danger mt-1">
                  {{ errors.email }}
                </div>
              </div>

              <!-- Password Field -->
              <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input 
                  type="password" 
                  class="form-control" 
                  id="password" 
                  v-model="formData.password"
                  @blur="() => validatePassword(true)"
                  @input="() => validatePassword(false)"
                >
                <div v-if="errors.password" class="text-danger mt-1">
                  {{ errors.password }}
                </div>
              </div>

              <!-- Submit Button -->
              <div class="text-center">
                <button type="submit" class="btn btn-success w-100 mb-2">Login / Register</button>
                <button type="button" class="btn btn-secondary w-100" @click="clearForm">Clear</button>
              </div>

              <div v-if="submittedSuccess" class="alert alert-success mt-3" role="alert">
                Success! Redirecting...
              </div>

            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { isAuthenticated } from '../store.js';

const router = useRouter();

const formData = ref({
  username: '',
  email: '',
  password: ''
});

const errors = ref({
  username: null,
  email: null,
  password: null
});

const submittedSuccess = ref(false);

const validateUsername = (blur) => {
  if (formData.value.username.length < 3) {
    if (blur) errors.value.username = "Name must be at least 3 characters";
  } else {
    errors.value.username = null;
  }
};

const validateEmail = (blur) => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(formData.value.email)) {
    if (blur) errors.value.email = "Please enter a valid email address.";
  } else {
    errors.value.email = null;
  }
};

const validatePassword = (blur) => {
  const password = formData.value.password;
  const minLength = 6;

  if (password.length < minLength) {
    if (blur) errors.value.password = `Password must be at least ${minLength} characters long.`;
  } else {
    errors.value.password = null;
  }
};

const submitForm = () => {
  // Trigger all validations
  validateUsername(true);
  validateEmail(true);
  validatePassword(true);

  if (!errors.value.username && !errors.value.email && !errors.value.password) {
    submittedSuccess.value = true;
    isAuthenticated.value = true; // Log user in
    setTimeout(() => {
      router.push('/resources');
    }, 500);
  } else {
    submittedSuccess.value = false;
  }
};

const clearForm = () => {
  formData.value = {
    username: '',
    email: '',
    password: ''
  };
  errors.value = {
    username: null,
    email: null,
    password: null
  };
  submittedSuccess.value = false;
};
</script>
