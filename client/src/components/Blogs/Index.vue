<template>
  <div class="blog-list">
    <div v-for="blog in blogs" :key="blog.id" class="blog-item">
      <img :src="BASE_URL + blog.thumbnail" alt="Blog Thumbnail" class="thumbnail" />
      <div class="blog-details">
        <p><strong>ชื่อ:</strong> {{ blog.title }}</p>
        <p><strong>รุ่นนิยม:</strong> {{ blog.category }}</p>
        <button @click="navigateTo(`/blog/${blog.id}`)">ดูรายละเอียด</button> 
      </div>
    </div>
  </div>
</template>

<script>
import BlogsService from '@/services/BlogsService'

export default {
  data() {
    return {
      blogs: [],
      BASE_URL: "http://localhost:8081/assets/uploads/",
    };
  },
  async created() {
    try {
      this.blogs = (await BlogsService.index()).data;
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
  },
};
</script>

<style scoped>
.blog-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center; /* จัดให้อยู่กึ่งกลาง */
  gap: 20px; /* เพิ่มระยะห่างระหว่างรายการ */
}

.blog-item {
  background-color: #ffffff;
  width: 250px;
  margin: 10px;
  padding: 15px;
  border: 2px solid #ddd; /* เพิ่มกรอบรอบรายการ */
  border-radius: 10px; /* ทำมุมให้โค้งมน */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* เพิ่มเงาให้กับรายการ */
  text-align: center;
  transition: transform 0.3s; /* เพิ่มการเคลื่อนไหวเมื่อโฮเวอร์ */
}

.blog-item:hover {
  transform: translateY(-5px); /* ขยับรายการขึ้นเมื่อโฮเวอร์ */
}

.thumbnail {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 8px; /* ทำมุมรูปภาพให้โค้งมน */
  margin-bottom: 10px;
  border: 1px solid #ddd; /* เพิ่มกรอบรอบรูปภาพ */
}

.blog-details {
  margin-top: 10px;
}

button {
  background-color: #f394db; /* สีพื้นหลัง */
  color: rgb(19, 21, 22); /* สีของข้อความ */
  border: none;
  padding: 10px 15px;
  text-align: center;
  font-size: 16px;
  margin: 4px 2px;
  border-radius: 8px; /* ทำมุมให้โค้งมน */
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s; /* เพิ่มการเคลื่อนไหวเมื่อเปลี่ยนสีและขยายปุ่ม */
}

button:hover {
  background-color: #8ae2fd; /* สีเมื่อวางเมาส์ */
  transform: scale(1.05); /* ขยายปุ่มเล็กน้อยเมื่อวางเมาส์ */
}
</style>
