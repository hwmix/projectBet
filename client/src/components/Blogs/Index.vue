<template>
  <div class="blog-list">
    <div v-for="blog in blogs" :key="blog.id" class="blog-item">
      <img :src="BASE_URL + blog.thumbnail" alt="Blog Thumbnail" class="thumbnail" />
      <p><strong>Title:</strong> {{ blog.title }}</p>
      <p><strong>Content:</strong> <span v-html="blog.content"></span></p>
      <p><strong>Category:</strong> {{ blog.category }}</p>
      <button @click="navigateTo(`/blog/${blog.id}`)">ดูรายละเอียด</button> 
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
}
.blog-item {
  width: 250px;
  margin: 10px;
  padding: 10px;
  border: 1px solid #ffffff;
  border-radius: 5px;
  text-align: center;
}
.thumbnail {
  width: 150px;
  height: 150px;
  object-fit: cover;
  margin-bottom: 10px;
}
</style>
