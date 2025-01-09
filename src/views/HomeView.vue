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

const handleUpdate = (id, updatedBlog) => {
	let index = state.blogs.map(x => {
		return x.id;
	}).indexOf(id);
	
	let newBlog = {
		id: id,
		title: updatedBlog.title,
		content: updatedBlog.content,
		tag: updatedBlog.tag
	};
	
	state.blogs.splice(index, 1);
	
	state.blogs = [
		...state.blogs.splice(0, index),
		newBlog,
		...state.blogs.splice(index)
	];
}
</script>

<template>
	<div class="container">
		<h1>NOBLOG</h1>
		<BlogForm @new-blog-created="addNewBlog" />
		<hr />
		<div class="loader" v-if="state.isLoading"></div>
		<Blog v-else @blog-deleted="handleDelete" @blog-updated="handleUpdate" v-for="blog in state.blogs" :key="blog.id" :blog="blog" />
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
		margin-bottom: 400px;
	}
	
	hr {
		margin-top: 50px;
		width: 35%;
	}
</style>
