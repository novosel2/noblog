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
	
	<footer class="footer">
		<p>&copy; 2025 Mateo Novosel. All rights reserved.</p>
	</footer>
</template>
	
<style scoped>
	h1 {
		margin-bottom: 40px;
		color: white;
		font-family: Arial, Helvetica, sans-serif;
	}
	
	.container {
		display: flex;
		flex: 1;
		align-items: center;
		flex-direction: column;
		height: fit-content;
		padding-bottom: 60px;
	}
	
	.footer {
		position: relative;
		bottom: 0;
		width: 100%;
		background-color: #363636;
		color: white;
		text-align: center;
		padding: 15px 0px 15px 0px;
		font-size: 14px;
		box-shadow: 1px 1px 10px #707070;
	}

	.footer p {
		margin: 0;
	}
	
	hr {
		margin-top: 50px;
		width: 45%;
		border-color: gray;
	}
</style>
