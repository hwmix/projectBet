<template>
    <div>
        <h1>Show Blog</h1>
        <p>id: {{ blog.id }}</p>
        <p>title: {{ blog.title }}</p>
        <!-- ใช้ v-html แทนการใช้ {{ blog.content }} -->
        <div v-html="blog.content"></div>
        <p>category: {{ blog.category }}</p>
        <p>status: {{ blog.status }}</p>
        <!-- แสดงภาพ thumbnail ถ้ามี -->
        <div v-if="blog.thumbnail && blog.thumbnail !== 'null'">
            <img :src="BASE_URL + blog.thumbnail" alt="Blog Thumbnail" style="max-width: 200px; max-height: 200px;" />
        </div>
        <p>
        <button v-on:click="navigateTo('/blog/edit/'+ blog.id)">แก้ไข blog</button>
        <button v-on:click="navigateTo('/blogs')">กลับ </button>
        </p>
    </div>
</template>

<script>
    import BlogsService from '@/services/BlogsService'
    export default {
        data () {
            return {
                blog: null,
                BASE_URL: "http://localhost:8081/assets/uploads/"
            }
        },
        async created () {
            try {
                let blogId = this.$route.params.blogId
                this.blog = (await BlogsService.show(blogId)).data
            } catch (error) {
                console.log (error)
            }
        },
        methods : {
            navigateTo (route) {
                this.$router.push(route)
            },
        }
    }
</script>

<style scoped>
</style>
