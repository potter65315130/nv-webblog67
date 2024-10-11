<template>
  <div class="container">
    <h1 class="title">Create User</h1>
    <form v-on:submit.prevent="createUser" class="form">
      <div class="form-group">
        <label>Name:</label>
        <input type="text" v-model="user.name" class="input" placeholder="Enter your name" />
      </div>
      <div class="form-group">
        <label>Lastname:</label>
        <input type="text" v-model="user.lastname" class="input" placeholder="Enter your lastname" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" v-model="user.email" class="input" placeholder="Enter your email" />
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" v-model="user.password" class="input" placeholder="Enter your password" />
      </div>
      <div class="button-container">
        <button type="submit" class="submit-button">Create User</button>
      </div>
    </form>
  </div>
</template>

<script>
import UsersService from '../../services/UsersService';

export default {
  data() {
    return {
      user: {
        name: '',
        lastname: '',
        email: '',
        password: '',
        status: 'active',
      },
    };
  },
  methods: {
    async createUser() {
      try {
        await UsersService.post(this.user);
        this.$router.push('/users');
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9e55b; /* Yellow background */
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  animation: fadeIn 0.5s ease;
}

.title {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
}

.input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  transition: border-color 0.3s ease;
}

.input:focus {
  border-color: #3498db; /* Change border color on focus */
  box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
}

.button-container {
  text-align: center;
}

.submit-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #3498db; /* Button color */
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.submit-button:hover {
  background-color: #2980b9; /* Darker button on hover */
  transform: scale(1.05); /* Slightly enlarge on hover */
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
