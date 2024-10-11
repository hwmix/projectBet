<template>
  <div class="show-blog-container">
    <div class="show-blog-content">
      <img :src="BASE_URL + blog.thumbnail" alt="Blog Thumbnail" class="blog-thumbnail" />
      <div class="blog-details">
        <p><strong>ชื่อ :</strong> {{ blog.title }}</p>
        <p><strong>รุ่นนิยม:</strong> {{ blog.category }}</p>
        <p><strong>รายละเอียด:</strong></p>
        <p v-html="blog.content"></p>
        <div class="button-container">
          <button @click="navigateTo('/blogs')" class="back-button">กลับหน้าหลัก</button>
          <div v-if="isUserLoggedIn && isAdmin">
            <button @click="navigateTo('/blog/edit/' + blog.id)" class="edit-button">แก้ไข</button>
            <button @click="deleteBlog(blog)" class="remove-button">ลบ Blog</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BlogsService from '@/services/BlogsService';

export default {
  data() {
    return {
      blog: {},
      BASE_URL: 'http://localhost:8081/assets/uploads/',
    };
  },
  async created() {
    try {
      let blogId = this.$route.params.blogId;
      this.blog = (await BlogsService.show(blogId)).data;
    } catch (error) {
      console.log(error);
    }
  },
  computed: {
    isUserLoggedIn() {
      return this.$store.state.user !== null;
    },
    isAdmin() {
      return this.$store.state.userType == "admin";
    },
    currentUser() {
      return this.$store.state.user;
    }
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteBlog(blog) {
      let result = confirm("Want to delete?");
      if (result) {
        try {
          await BlogsService.delete(blog);
          this.refreshData();
          this.$router.push({ name: 'blogs' });
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      this.blogs = (await BlogsService.index()).data;
    }
  }
};
</script>

<style scoped>
.show-blog-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.show-blog-content {
  display: flex;
  background-color: #ffffff;
  padding: 20px;
  border-radius: 10px; /* ทำให้มุมโค้งมน */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* เพิ่มเงาเพื่อให้ดูเหมือนกรอบยกสูง */
  border: 2px solid #ddd; /* เพิ่มกรอบรอบเนื้อหา */
  max-width: 800px; /* กำหนดความกว้างสูงสุดเพื่อให้ดูเป็นระเบียบ */
  flex-wrap: wrap; /* ให้มีการห่อเนื้อหาในกรณีที่หน้าจอแคบ */
}

.blog-thumbnail {
  width: 250px;
  height: 250px;
  object-fit: cover;
  margin-right: 20px;
  border-radius: 10px; /* ทำให้รูปภาพมีมุมโค้งมน */
  border: 1px solid #ddd; /* เพิ่มกรอบรอบรูปภาพ */
}

.blog-details {
  display: flex;
  flex-direction: column;
  flex: 1; /* ให้พื้นที่เนื้อหายืดตามขนาดที่มี */
}

.button-container {
  margin-top: 20px;
  display: flex;
  gap: 10px;
  flex-wrap: wrap; /* จัดเรียงปุ่มให้ห่อเมื่อหน้าจอแคบ */
}

button {
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  color: white;
  transition: background-color 0.3s;
}

.back-button {
  background-color: #f0ad4e;
}

.back-button:hover {
  background-color: #ec971f; /* สีของปุ่มเมื่อวางเมาส์ */
}

.edit-button {
  background-color: #5cb85c;
}

.edit-button:hover {
  background-color: #4cae4c; /* สีของปุ่มเมื่อวางเมาส์ */
}

.remove-button {
  background-color: #e4523e;
}

.remove-button:hover {
  background-color: #c9302c; /* สีของปุ่มเมื่อวางเมาส์ */
}
</style>
