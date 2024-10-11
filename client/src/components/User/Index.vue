<template>
  <div class="user-container">
    <h1>Get All Users</h1>
    <div class="header-actions">
      <button class="btn-create" v-on:click="navigateTo('/user/create')">สร้างผู้ใช้</button>
    </div>
    <hr>
    <div v-if="users.length">
      <div class="user-count"><b>จำนวนผู้ใช้งาน:</b> {{ users.length }}</div>
      <div class="user-item" v-for="user in users" v-bind:key="user.id">
        <div><b>id:</b> {{ user.id }}</div>
        <div><b>ชื่อผู้ใช้:</b> {{ user.name }} {{ user.lastname }}</div>
        <div><b>อีเมล:</b> {{ user.email }}</div>
        <div><b>status:</b> {{ user.status }}</div>
        <div><b>type:</b> {{ user.type }}</div>
        <div class="user-actions">
          <button class="btn-view" v-on:click="navigateTo('/user/'+user.id)">ดูข้อมูล</button>
          <button class="btn-edit" v-on:click="navigateTo('/user/edit/'+user.id)">แก้ไขข้อมูล</button>
          <button class="btn-delete" v-on:click="deleteUser(user)">ลบข้อมูล</button>
        </div>
        <hr>
      </div>
    </div>
    <div><button class="btn-logout" v-on:click="logout">Logout</button></div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";
export default {
  data() {
    return {
      users: []
    };
  },
  async created() {
    try {
      this.users = (await UsersService.index()).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    logout() {
      this.$store.dispatch('setToken', null);
      this.$store.dispatch('setUser', null);
      this.$router.push({ name: 'login' });
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(user) {
      let result = confirm("คุณต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        try {
          await UsersService.delete(user);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>

<style scoped>
/* Container for user management page with yellow background */
.user-container {
  width: 80%;
  margin: 0 auto;
  padding: 30px;
  background-color: #f9e55b; /* Yellow background */
  border-radius: 15px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.2);
  font-family: 'Roboto', sans-serif;
  transition: transform 0.3s ease;
}

/* Header actions styling */
.header-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

h1 {
  color: #2c3e50;
}

.user-count {
  font-size: 1.2em;
  margin-bottom: 20px;
  color: #2c3e50;
}

/* Individual user item styling */
.user-item {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.user-item:hover {
  transform: scale(1.02);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

/* User actions buttons */
.user-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.btn-view,
.btn-edit,
.btn-delete {
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn-view {
  background-color: #1abc9c;
  color: white;
}

.btn-view:hover {
  background-color: #16a085;
}

.btn-edit {
  background-color: #f39c12;
  color: white;
}

.btn-edit:hover {
  background-color: #e67e22;
}

.btn-delete {
  background-color: #e74c3c;
  color: white;
}

.btn-delete:hover {
  background-color: #c0392b;
}

/* Create user button */
.btn-create {
  padding: 10px 20px;
  background-color: #4a69bd;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn-create:hover {
  background-color: #6a89cc;
  transform: translateY(-2px);
}

/* Logout button */
.btn-logout {
  padding: 10px 15px;
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-logout:hover {
  background-color: #c0392b;
}

/* Responsive design */
@media (max-width: 768px) {
  .user-container {
    width: 95%;
    padding: 20px;
  }

  h1 {
    font-size: 1.8em;
  }
}
</style>
