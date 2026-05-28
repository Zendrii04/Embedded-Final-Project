<script setup>
import { ref } from 'vue'
import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword } from 'firebase/auth'

const email = ref('')
const password = ref('')
const errorMessage = ref('')
const isLoading = ref(false)
const isSignUp = ref(false)
const showPassword = ref(false)

const handleSubmit = async () => {
  if (!email.value || !password.value) {
    errorMessage.value = 'Please enter both email and password.'
    return
  }

  isLoading.value = true
  errorMessage.value = ''
  
  const auth = getAuth()
  try {
    if (isSignUp.value) {
      await createUserWithEmailAndPassword(auth, email.value, password.value)
    } else {
      await signInWithEmailAndPassword(auth, email.value, password.value)
    }
  } catch (error) {
    console.error("Auth failed", error)
    if (isSignUp.value) {
      errorMessage.value = 'Failed to create account. Email might be in use or password too weak.'
    } else {
      errorMessage.value = 'Invalid email or password. Please try again.'
    }
  } finally {
    isLoading.value = false
  }
}
</script>

<template>
  <div class="overlay">
    <div class="login-box">
      <h2>E-Waste Dashboard</h2>
      <p>{{ isSignUp ? 'Create a new account' : 'Please log in to access the control panel.' }}</p>
      
      <form @submit.prevent="handleSubmit">
        <div class="input-group">
          <label>Email</label>
          <input type="email" v-model="email" placeholder="admin@example.com" required />
        </div>
        
        <div class="input-group">
          <label>Password</label>
          <div class="password-wrapper">
            <input :type="showPassword ? 'text' : 'password'" v-model="password" placeholder="********" required />
            <button type="button" class="toggle-password" @click="showPassword = !showPassword" title="Toggle Password Visibility">
              <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path><circle cx="12" cy="12" r="3"></circle></svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path><line x1="1" y1="1" x2="23" y2="23"></line></svg>
            </button>
          </div>
        </div>

        <p v-if="errorMessage" class="error-msg">{{ errorMessage }}</p>

        <button type="submit" class="submit-btn" :disabled="isLoading">
          <span v-if="isLoading">{{ isSignUp ? 'Creating account...' : 'Logging in...' }}</span>
          <span v-else>{{ isSignUp ? 'Sign Up' : 'Log In' }}</span>
        </button>
      </form>
      
      <p class="toggle-mode">
        {{ isSignUp ? 'Already have an account?' : 'Need an account?' }}
        <a href="#" @click.prevent="isSignUp = !isSignUp; errorMessage = ''">
          {{ isSignUp ? 'Log In' : 'Sign Up' }}
        </a>
      </p>
    </div>
  </div>
</template>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(15, 23, 42, 0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(8px);
}

.login-box {
  background: #1e293b;
  padding: 40px 35px;
  border-radius: 16px;
  box-shadow: 0 20px 40px rgba(0,0,0,0.4);
  width: 100%;
  max-width: 400px;
  text-align: center;
  border: 1px solid #334155;
}

.login-box h2 {
  margin-top: 0;
  color: #f8fafc;
  font-size: 1.8em;
}

.login-box p {
  color: #94a3b8;
  margin-bottom: 25px;
}

.input-group {
  margin-bottom: 20px;
  text-align: left;
}

.input-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #cbd5e1;
  font-size: 0.9em;
}

.password-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.password-wrapper input {
  padding-right: 45px;
}

.toggle-password {
  position: absolute;
  right: 5px;
  background: transparent;
  border: none;
  color: #64748b;
  cursor: pointer;
  padding: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: color 0.2s;
}

.toggle-password:hover {
  color: #cbd5e1;
}

input {
  width: 100%;
  padding: 12px 15px;
  background-color: #0f172a;
  border: 1px solid #334155;
  border-radius: 8px;
  font-size: 1em;
  color: #f8fafc;
  box-sizing: border-box;
  transition: all 0.2s ease;
}

input:focus {
  border-color: #38bdf8;
  box-shadow: 0 0 0 3px rgba(56, 189, 248, 0.2);
  outline: none;
}

input::placeholder {
  color: #475569;
}

.submit-btn {
  width: 100%;
  padding: 14px;
  background-color: #38bdf8;
  color: #0f172a;
  border: none;
  border-radius: 8px;
  font-size: 1.05em;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  margin-top: 10px;
}

.submit-btn:hover:not(:disabled) {
  background-color: #0ea5e9;
  transform: translateY(-1px);
}

.submit-btn:disabled {
  background-color: #0ea5e9;
  opacity: 0.7;
  cursor: not-allowed;
}

.error-msg {
  color: #ef4444 !important;
  font-size: 0.85em;
  margin-top: 0;
  margin-bottom: 15px;
  background: rgba(239, 68, 68, 0.1);
  padding: 10px;
  border-radius: 6px;
  border: 1px solid rgba(239, 68, 68, 0.2);
}

.toggle-mode {
  margin-top: 25px !important;
  font-size: 0.9em;
  color: #94a3b8 !important;
}

.toggle-mode a {
  color: #38bdf8;
  font-weight: 600;
  text-decoration: none;
  margin-left: 5px;
  transition: color 0.2s;
}

.toggle-mode a:hover {
  color: #7dd3fc;
  text-decoration: underline;
}
</style>
