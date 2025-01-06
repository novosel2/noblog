<script setup>
import { reactive } from 'vue';
import { defineEmits } from 'vue';
import axios from 'axios';

const emit = defineEmits();

const form = reactive({
	title: '',
	tag: 'General',
	content: ''
});

const handleSubmit = async () => {
	const newBlog = {
		title: form.title,
		content: form.content.replace(/(\r\n|\n|\r)/g, ' '),
		tag: form.tag
	};
	
	form.title = '';
	form.content = '';
	form.tag = 'General';
	
	try {
		await axios.post('https://localhost:8000/api/articles/create-article', newBlog);
		emit('new-blog-created', newBlog);
	} catch (error) {
		console.error('Error adding new blog', error);
	}
}
</script>

<template>
	<form @submit.prevent="handleSubmit">
		<div class="form">
			<label>Title: </label>
			<input v-model="form.title" placeholder="Working on a new project" required/> <br />
			<label>Tag: </label>
			<select v-model="form.tag">
				<option>General</option>
				<option>Work</option>
				<option>Hobby</option>
				<option>Food</option>
				<option>Entertainment</option>
			</select>
			<textarea v-model="form.content" placeholder="What's going on?" required></textarea><br />
			<button style="float: right" type="submit">Post</button>
		</div>
	</form>
</template>

<style>
	.form {
		width: 500px;
	}

	input {
		width: 300px; 
		margin-bottom: 10px
	}
	
	textarea {
		width: 100%; 
		height: 100px
	}
	
	select {
		width: 120px; 
		margin-bottom: 10px
	}
</style>