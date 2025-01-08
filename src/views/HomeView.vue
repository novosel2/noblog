<script setup>
import BlogForm from '@/components/BlogForm.vue';
import Blog from '@/components/Blog.vue';
import PopoutBlog from '@/components/PopoutBlog.vue';
import { reactive, onMounted } from 'vue';
import axios from 'axios';

const state = reactive({
	blogs: [],
	isLoading: true
});

var blog = reactive({
	id: '',
	title: '',
	tag: '',
	content: ''
});

onMounted(async () => {
	try {
		const response = await axios.get('https://localhost:8000/api/articles');
		state.blogs = response.data;
	} catch (error) {
		console.error('Error while getting all blogs', error);
	} finally {
		state.isLoading = false;
	}
});

const addNewBlog = async () => {
	try {
		const response = await axios.get('https://localhost:8000/api/articles');
		state.blogs = response.data;
	} catch (error) {
		console.error('Error while getting all blogs', error);
	}
}

const handleDelete = (id) => {
	let index = state.blogs.map(x => {
		return x.id;
	}).indexOf(id);
	
	state.blogs.splice(index, 1);
}
</script>

<template>
	<div class="container">
		<h1>NOBLOG</h1>
		<BlogForm @new-blog-created="addNewBlog" />
		<hr />
		<div class="loader" v-if="state.isLoading"></div>
		<Blog v-else @blog-deleted="handleDelete" v-for="blog in state.blogs" :key="blog.id" :id="blog.id" :title="blog.title" :content="blog.content" :tag="blog.tag" />
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
