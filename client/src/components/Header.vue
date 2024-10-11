<template>
  <div>
    <div class="nv-navbar">
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">Blogs</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>
        
        <!-- แสดง Logout เมื่อผู้ใช้ล็อกอิน -->
        <li v-if="isUserLoggedIn">
          <a v-if="isAdmin" href="#"  v-on:click="navigateTo('/blog/create')">AddItem</a>
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
</script>

<style scoped>
.nv-navbar {
  background-color: rgb(245, 195, 230);
  width: 100%;
  height: 20px;
  padding: 10px 0px 10px 0px;
}
.nv-navbar .nav {
  list-style: none;
  margin: 0;
  padding: 0;
  float: left;
}
.nv-navbar .nav li {
  float: left;
}
.nv-navbar .nav li a {
  padding: 10px;
  text-decoration: none;
  color: rgb(150, 100, 134);
  font-weight: bold;
}
.nv-navbar .nav li a:hover {
  padding: 10px;
  text-decoration: none;
  color: rgb(225, 236, 236);
}
.nv-navbar .nav li a.router-link-active {
  background-color: rgba(228, 182, 209, 0.973);
  color: rgb(236, 66, 148);
}
/* ทำให้ปุ่ม Logout ชิดขวา */
.nv-navbar .nav li:last-child {
  float: right;
}
.clearfix {
  clear: left;
}
</style>
