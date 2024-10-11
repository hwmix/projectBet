<template>
  <div class="user-detail-container">
    <div class="user-card">
      <h1>Show User</h1>
      <p><b>ID:</b> {{ user.id }}</p>
      <p><b>Name:</b> {{ user.name }}</p>
      <p><b>Lastname:</b> {{ user.lastname }}</p>
      <p><b>Email:</b> {{ user.email }}</p>
     
      <p><b>Type:</b> {{ user.type }}</p>
      <p><b>Created At:</b> {{ user.createdAt }}</p>

      <!-- ปุ่มกลับไปยังหน้าก่อนหน้า -->
      <button class="back-button" @click="goBack">กลับ</button>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default { 
  data() {
    return {
      user: {}
    }
  },
  async created() {
    try {
      var userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch(err) {
      console.log(err);
    }
  },
  methods: {
    goBack() {
      this.$router.go(-1); // กลับไปยังหน้าก่อนหน้า
    }
  }
}
</script>

<style scoped>
.user-detail-container {
  display: flex;
  justify-content: center; /* จัดให้อยู่กึ่งกลางในแนวนอน */
  align-items: center; /* จัดให้อยู่กึ่งกลางในแนวตั้ง */
  height: 100vh; /* ให้มีความสูงเต็มหน้าจอ */
  background-color: #f3f4f6; /* สีพื้นหลังที่นุ่มนวล */
}

.user-card {
  background-color: white; /* พื้นหลังของ card */
  padding: 20px;
  border-radius: 10px; /* มุมโค้งของ card */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* เพิ่มเงาให้กับ card */
  width: 350px; /* กำหนดความกว้างของ card */
  text-align: center; /* จัดข้อความให้อยู่กลาง */
}

.user-card h1 {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

.user-card p {
  margin: 10px 0;
  color: #555;
}

.back-button {
  background-color: #2196F3; /* สีพื้นหลังของปุ่มกลับ */
  color: white;
  border: none;
  padding: 10px 20px;
  margin-top: 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.back-button:hover {
  background-color: #1976D2; /* สีของปุ่มกลับเมื่อวางเมาส์ */
}
</style>
