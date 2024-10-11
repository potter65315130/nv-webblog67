<template>
  <div class="login-page">
    <div class="login-box" @mouseover="hover = true" @mouseleave="hover = false">
      <h1>Welcome</h1>
      <p>Please log in to your account</p>
      <form v-on:submit.prevent="onLogin" class="login-form">
        <div class="form-group">
          <label for="email">Email</label>
          <input 
            type="email" 
            id="email" 
            name="email" 
            v-model="email" 
            required 
            placeholder="Enter your email"
          />
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            v-model="password"
            required
            placeholder="Enter your password"
          />
        </div>
        <button type="submit" class="btn-login">Login</button>
        <div class="error" v-if="error">{{ error }}</div>
      </form>
    </div>
  </div>
</template>

<script>
import AuthenService from "../services/AuthenService";
export default {
  data() {
    return {
      email: "",
      password: "",
      error: null,
      hover: false, // Track hover state
    };
  },
  methods: {
    async onLogin() {
      try {
        const response = await AuthenService.login({
          email: this.email,
          password: this.password,
        });

        this.$store.dispatch("setToken", response.data.token);
        this.$store.dispatch("setUser", response.data.user);

        this.$router.push({
          name: "users",
        });
      } catch (error) {
        console.log(error);
        this.error = error.response.data.error;
        this.email = "";
        this.password = "";
      }
    },
  },
};
</script>

<style scoped>
/* Container for the full page */
.login-page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f9e55b; /* Updated to yellow background */
  font-family: 'Roboto', sans-serif;
}

/* Login box styling */
.login-box {
  background-color: rgba(255, 255, 255, 0.9);
  padding: 40px;
  border-radius: 15px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.2);
  text-align: center;
  width: 400px;
  max-width: 90%;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  animation: fadeIn 0.5s; /* Added fade-in animation */
}

/* Heading styles */
h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  color: #2c3e50;
  font-weight: 700;
  animation: bounce 1s infinite; /* Bouncing effect */
}

/* Subtitle styles */
p {
  color: #7f8c8d;
  margin-bottom: 30px;
  font-size: 1.1em;
}

/* Form styles */
.login-form {
  display: flex;
  flex-direction: column;
}

/* Form group styling */
.form-group {
  margin-bottom: 20px;
  text-align: left;
}

/* Input field styling */
input {
  width: 100%;
  padding: 12px;
  border: 2px solid #ccc;
  border-radius: 8px;
  margin-top: 8px;
  font-size: 16px;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input:focus {
  border-color: #4a69bd;
  outline: none;
  box-shadow: 0 0 5px rgba(74, 105, 189, 0.5);
}

/* Button styling */
.btn-login {
  width: 100%;
  padding: 12px;
  background-color: #4a69bd;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s ease, transform 0.2s ease;
  margin-top: 10px;
}

.btn-login:hover {
  background-color: #6a89cc;
  transform: translateY(-2px);
}

/* Error message styling */
.error {
  color: #e74c3c;
  margin-top: 15px;
  font-weight: bold;
}

/* Hover effect on login box */
.login-box:hover {
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.3);
  transform: scale(1.02); /* Slightly enlarge on hover */
}

/* Animations */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

/* Responsive design */
@media (max-width: 768px) {
  .login-box {
    width: 90%;
    padding: 20px;
  }

  h1 {
    font-size: 2em;
  }
}
</style>
