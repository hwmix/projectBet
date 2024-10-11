<template>
  <div>
    <h1>All Users</h1>
    <div>
      <!-- <button v-on:click="navigateTo('/user/create')">สร้างผู้ใช้</button> -->
    </div>
    <hr>
    <div v-if="users.length">
      <div><b>จำนวนผู้ใช้งาน:</b> {{ users.length }}</div>
      <div v-for="userItem in users" v-bind:key="userItem.id" class="user-item-container">
        <div class="user-item-header"><b>ชื่อผู้ใช้:</b> {{ userItem.name }} {{ userItem.lastname }}</div>
        <div><b>อีเมล:</b> {{ userItem.email }}</div>
        <div><b>ประเภทผู้ใช้:</b> {{ userItem.type }}</div>
        <div class="button-group">
          <button class="view-button" v-on:click="navigateTo('/user/'+userItem.id)">ดูข้อมูล</button>
          <!-- แสดงปุ่มแก้ไข/ลบถ้าผู้ใช้เป็น admin -->
          <div v-if="user && user.type === 'admin'">
            <button class="edit-button" v-on:click="navigateTo('/user/edit/'+userItem.id)">แก้ไขข้อมูล</button>
            <button class="delete-button" v-on:click="deleteUser(userItem)">ลบข้อมูล</button>
          </div>
        </div>
        <hr>
      </div>
    </div>

    <!-- ปุ่ม Logout จะแสดงเมื่อมีการล็อกอิน -->
    <div v-if="user">
      <!-- <button v-on:click="logout">Logout</button> -->
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      users: [],
      user: null // เก็บข้อมูลผู้ใช้ปัจจุบัน
    };
  },
  async created() {
    await this.refreshData(); // โหลดข้อมูลผู้ใช้
    this.user = this.$store.state.user; // ดึงข้อมูลผู้ใช้ปัจจุบันจาก store
  },
  methods: {
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
        this.user = this.$store.state.user; // ดึงข้อมูลผู้ใช้จาก store ทุกครั้งที่ refresh
      } catch (err) {
        console.log(err);
      }
    },
    logout() {
      this.$store.dispatch('setToken', null);
      this.$store.dispatch('setUser', null);
      this.$router.push({ name: 'login' });
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(userItem) {
      let result = confirm("คุณต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        try {
          await UsersService.delete(userItem);
          await this.refreshData(); // refresh ข้อมูลหลังจากลบผู้ใช้สำเร็จ
        } catch (err) {
          console.log(err);
        }
      }
    }
  }
};
</script>

<style scoped>
h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.user-item-container {
  border: 1px solid #e0e0e0; /* กรอบรอบแต่ละผู้ใช้ */
  padding: 15px;
  border-radius: 8px;
  background-color: #f9f9f9; /* สีพื้นหลังที่นุ่มนวล */
  margin-bottom: 10px;
}

.user-item-header {
  font-weight: bold;
  font-size: 16px;
  color: #333;
}

.button-group {
  margin-top: 10px;
}

button {
  background-color: #4CAF50; /* สีพื้นหลังของปุ่ม */
  color: white; /* สีของข้อความในปุ่ม */
  border: none;
  padding: 10px 15px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button.view-button {
  background-color: #2196F3; /* สีพื้นหลังของปุ่มดูข้อมูล */
}

button.view-button:hover {
  background-color: #1976D2; /* สีของปุ่มดูข้อมูลเมื่อวางเมาส์ */
}

button.edit-button {
  background-color: #FFC107; /* สีพื้นหลังของปุ่มแก้ไข */
}

button.edit-button:hover {
  background-color: #FFB300; /* สีของปุ่มแก้ไขเมื่อวางเมาส์ */
}

button.delete-button {
  background-color: #f44336; /* สีพื้นหลังของปุ่มลบ */
}

button.delete-button:hover {
  background-color: #d32f2f; /* สีของปุ่มลบเมื่อวางเมาส์ */
}

hr {
  border: 1px solid #ddd;
  margin: 15px 0;
}
</style>
