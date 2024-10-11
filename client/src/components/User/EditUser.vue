<template>
  <div class="edit-user-container">
    <h1>Edit User</h1>
    <form v-on:submit.prevent="editUser" class="edit-user-form">
      <div class="form-group">
        <label>Name:</label>
        <input type="text" v-model="user.name" />
      </div>
      <div class="form-group">
        <label>Lastname:</label>
        <input type="text" v-model="user.lastname" />
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" v-model="user.email" />
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" v-model="user.password" />
      </div>
      <div class="form-group">
        <button type="submit" class="btn-submit">Edit User</button>
      </div>
    </form>
  </div>
</template>

<script>
import UsersService from "../../services/UsersService";
export default {
  data() {
    return {
      user: {
        name: "",
        lastname: "",
        email: "",
        password: "",
        status: "active"
      }
    };
  },
  async created() {
    try {
      var userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    async editUser() {
      try {
        await UsersService.put(this.user);
        this.$router.push("/users");
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>

<style scoped>
/* สไตล์สำหรับพื้นหลังสีเหลือง */
.edit-user-container {
  background-color: #f9e55b;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  max-width: 600px;
  margin: 40px auto;
  animation: fadeIn 1s ease-in-out;
}

/* สไตล์ของฟอร์มและช่องกรอกข้อมูล */
.edit-user-form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
  color: #333;
}

input {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  width: 100%;
  transition: box-shadow 0.3s ease;
}

input:focus {
  box-shadow: 0 0 8px rgba(255, 221, 87, 0.8);
}

/* ปุ่มส่งข้อมูล */
.btn-submit {
  background-color: #3498db;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.btn-submit:hover {
  background-color: #2980b9;
  transform: translateY(-3px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

/* แอนิเมชันแบบ fade-in */
@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(-20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
