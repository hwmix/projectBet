<template>
  <div>
    <div class="nv-navbar">
      <li class="logo-container">
          <img src="@/assets/E.jpg" alt="Logo" class="logo-image" height="50px"/>
        </li>
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">Fishing Rod</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>

        <!-- แสดง Logout เมื่อผู้ใช้ล็อกอิน -->
        <li v-if="isUserLoggedIn">
          <a v-if="isAdmin" href="#" v-on:click="navigateTo('/blog/create')">Add Rod</a>
          <a href="#" v-on:click.prevent="logout">Logout</a>
        </li>

        <!-- แสดง Login เมื่อยังไม่ล็อกอิน -->
        <li v-else>
          <router-link :to="{ name: 'login' }">Login</router-link>
        </li>
      </ul>
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
  computed: {
    // ใช้ computed เพื่อดึงค่าจาก store
    isUserLoggedIn() {
      return this.$store.state.user !== null;
    },

    isAdmin() {
      return this.$store.state.userType == "admin"
    },
    currentUser() {
      return this.$store.state.user;
    }
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
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
  }
};
//css
</script>

<style scoped>
.nv-navbar {
  background-color: rgb(245, 195, 230);
  width: 100%;
  padding: 10px 20px; /* เพิ่มระยะขอบในแถบเมนูให้ดูมีพื้นที่มากขึ้น */
  display: flex; /* ใช้ Flexbox สำหรับการจัดเรียง */
  justify-content: space-between; /* แบ่งระยะห่างระหว่างรายการเมนูซ้ายและขวา */
  align-items: center; /* จัดตำแหน่งให้กึ่งกลางในแนวตั้ง */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* เพิ่มเงาเล็กน้อยเพื่อให้เมนูดูโดดเด่น */
}

.nv-navbar .nav {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nv-navbar .nav li {
  margin-right: 15px; /* เพิ่มระยะห่างระหว่างรายการเมนูให้ดูกว้างขึ้น */
}

.nv-navbar .nav li a {
  padding: 8px 16px; /* ปรับขนาดของปุ่มในเมนูให้สมส่วนมากขึ้น */
  text-decoration: none;
  color: rgb(150, 100, 134);
  font-weight: bold;
  border-radius: 5px; /* เพิ่มความโค้งมนของปุ่มเพื่อความสวยงาม */
  transition: background-color 0.3s, color 0.3s; /* เพิ่มการเคลื่อนไหวเมื่อมีการเปลี่ยนสี */
}

.nv-navbar .nav li a:hover {
  background-color: rgba(228, 182, 209, 0.8); /* เปลี่ยนสีพื้นหลังเมื่อวางเมาส์ */
  color: white; /* เปลี่ยนสีของข้อความเมื่อวางเมาส์ */
}

.nv-navbar .nav li a.router-link-active {
  background-color: rgba(228, 182, 209, 0.973);
  color: rgb(236, 66, 148);
}

/* ทำให้ปุ่ม Logout หรือรายการสุดท้ายในเมนูชิดขวา */
.nv-navbar .nav li:last-child {
  margin-left: auto; /* ใช้ margin-left: auto เพื่อดันรายการนี้ไปทางขวาสุด */
}

/* ทำให้แถบเมนูปรับขนาดตามหน้าจอ */
@media (max-width: 768px) {
  .nv-navbar {
    flex-direction: column; /* เปลี่ยนการจัดเรียงเป็นแนวตั้งเมื่อหน้าจอเล็กลง */
    align-items: flex-start; /* จัดแนวให้เริ่มจากด้านซ้าย */
  }

  .nv-navbar .nav {
    flex-direction: column; /* เรียงรายการเมนูในแนวตั้ง */
    width: 100%; /* ให้เมนูยืดขนาดเต็มของหน้าจอ */
  }

  .nv-navbar .nav li {
    margin: 5px 0; /* ปรับระยะห่างระหว่างรายการเมนู */
  }
}
.nv-navbar {
  background-color: rgb(245, 195, 230);
  width: 100%;
  padding: 10px 20px;
  display: flex; /* ใช้ Flexbox สำหรับการจัดเรียง */
  justify-content: space-between; /* แบ่งระยะห่างระหว่างรายการเมนูซ้ายและขวา */
  align-items: center; /* จัดตำแหน่งให้กึ่งกลางในแนวตั้ง */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* จัดตำแหน่งไอคอนให้ไปอยู่ทางซ้ายสุด */
.icon-container {
  display: flex;
  align-items: center;
  margin-right: auto; /* จัดตำแหน่งไอคอนไปอยู่ทางซ้ายสุด */
}

.header-icon {
  width: 32px; /* ขนาดของไอคอนรูปภาพ */
  height: 32px;
  border-radius: 50%; /* ทำให้รูปภาพเป็นวงกลม */
  cursor: pointer;
}

.nv-navbar .nav {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

</style>
