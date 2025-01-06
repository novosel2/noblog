<script setup>
import BlogForm from '@/components/BlogForm.vue';
import Blog from '@/components/Blog.vue';
import { reactive, onMounted } from 'vue';
import axios from 'axios';

const state = reactive({
	blogs: [],
	isLoading: true
});

onMounted(async () => {
	try {
		const response = await axios.get('https://localhost:8000/api/articles');
		state.blogs = response.data;
	} catch (error) {
		console.error('Error while getting all blogs', error);
	} finally {
		state.isLoading = true;
	}
});

const addNewBlog = (newBlog) => {
	console.log(newBlog);
	state.blogs.unshift(newBlog);
}
</script>

<template>
	<div class="container">
		<h1>NOBLOG</h1>
		<BlogForm @new-blog-created="addNewBlog" />
		<hr />
		<Blog v-for="blog in state.blogs" :key="blog.content" :title="blog.title" :content="blog.content" :tag="blog.tag" />
	</div>
</template>

<style scoped>
	h1 {
		margin-bottom: 40px;
	}

	.container {
		display: flex;
		align-items: center;
		flex-direction: column;
		height: 100vh;
	}
	
	hr {
		margin-top: 50px;
		width: 35%;
	}
</style>
