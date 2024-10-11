<template>
  <div class="user-details">
    <h1>Show User</h1>
    <div class="user-card">
      <p><strong>ID:</strong> {{ user.id }}</p>
      <p><strong>Name:</strong> {{ user.name }}</p> <!-- แก้จาก user.id เป็น user.name -->
      <p><strong>Lastname:</strong> {{ user.lastname }}</p>
      <p><strong>Email:</strong> {{ user.email }}</p>
      <p><strong>Password:</strong> {{ user.password }}</p>
      <p><strong>Status:</strong> {{ user.status }}</p>
      <p><strong>Type:</strong> {{ user.type }}</p>
      <p><strong>Created At:</strong> {{ user.createdAt }}</p>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      user: {}
    };
  },
  async created() {
    try {
      var userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch (err) {
      console.log(err);
    }
  }
};
</script>

<style scoped>
/* สไตล์สำหรับพื้นหลังสีเหลือง */
.user-details {
  background-color: #f9e55b; /* สีเหลือง */
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  max-width: 600px;
  margin: 40px auto;
  animation: fadeIn 1s ease-in-out; /* เพิ่มแอนิเมชัน */
}

/* การตั้งค่าสำหรับการ์ดแสดงข้อมูล */
.user-card {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: left; /* จัดข้อความชิดซ้าย */
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* เอฟเฟกต์ hover บนการ์ด */
.user-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

/* การตั้งค่าสำหรับข้อความ */
p {
  font-size: 16px;
  color: #333;
  margin: 10px 0;
  text-align: left; /* จัดข้อความชิดซ้าย */
}

/* แอนิเมชันแบบ fade in */
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
