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
		emit('new-blog-created');
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
			<button class="btn-post" type="submit">Post</button>
		</div>
	</form>
</template>

<style>
	.form {
		width: 500px;
		background-color: #363636;
		padding: 40px 40px 40px 40px;
		color: white;
		box-shadow: 1px 1px 10px #707070;
	}

	input {
		width: 300px; 
		margin-bottom: 10px;
		background-color: #444;
		border: 2px black inset;
		color: white;
	}
	
	input::placeholder {
		color: #BBBBBB;
	}
	
	textarea {
		width: 100%; 
		height: 100px;
		padding: 5px 0 0 5px;
		background-color: #444;
		border: 2px black inset;
		color: white;
		resize: none;
	}
	
	textarea::placeholder {
		color: #BBBBBB;
	}
	
	textarea:focus {
		outline:  none;
	}
	
	select {
		width: 120px; 
		margin-bottom: 10px;
		background-color: #444;
		color: white;
		border: 2px black inset;
	}
	
	label {
		display: inline-block;
		width: 40px;
	}
	
	.btn-post {
		float: right;
		background-color: #444;
		color: white;
		width: 60px;
		height: 24px;
		border: 2px black outset;
		margin-top: 5px;
	}
</style>