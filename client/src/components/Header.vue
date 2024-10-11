<template>
  <div>
    <nav class="navbar">
      <div class="logo" @click="navigateToBlogs">
        <img src="https://files.oaiusercontent.com/file-myiyGPtw3Zv6Fg3qpVIGwGB9?se=2024-10-11T13%3A23%3A08Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D604800%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3Da29b14bb-fa9c-4171-8055-14739194467d.webp&sig=f/XVXQs4ZZj5JTKhN4HnY%2B9MtT4PH%2BjTGgkCUfdosm8%3D" alt="School Logo" /> <!-- Update with your school logo path -->
      </div>
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">Blogs</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>
        <li v-if="!isLogin()"><router-link :to="{ name: 'login' }">Login</router-link></li>
        <li v-if="isLogin()"><a @click.prevent="logout">Logout</a></li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  methods: {
    isLogin() {
      return this.$store.getters.isUserLoggedIn;
    },
    logout() {
      this.$store.dispatch('logout');
      this.$router.push({ name: 'login' });
    },
    navigateToBlogs() {
      this.$router.push({ name: 'blogs' }); // Redirect to the school information page
    }
  }
};
</script>

<style scoped>
.navbar {
  background-color: #282c34;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  position: relative;
  padding: 15px 40px; /* Increased padding for width */
  transition: background-color 0.3s ease;
  display: flex;
  justify-content: space-between; /* Adjusted to space between logo and nav items */
  align-items: center; /* Center items vertically */
}

.logo {
  cursor: pointer; /* Change cursor to pointer */
  transition: transform 0.3s;
}

.logo img {
  height: 40px; /* Adjust height as needed */
  width: auto; /* Maintain aspect ratio */
}

.logo:hover {
  transform: scale(1.1); /* Logo scaling effect */
}

.nav {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  gap: 60px; /* Increased spacing between items */
}

.nav li {
  position: relative;
}

.nav li a {
  padding: 10px 30px; /* Wider padding for links */
  text-decoration: none;
  color: #ffffff;
  border-radius: 5px;
  transition: all 0.3s;
  position: relative;
}

.nav li a:hover {
  color: #ffdd57;
  transform: translateY(-3px);
  box-shadow: 0 4px 12px rgba(255, 221, 87, 0.5);
}

.nav li a::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 3px;
  background: #ffdd57;
  left: 0;
  bottom: -3px;
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.nav li a:hover::after {
  transform: scaleX(1);
}
</style>
