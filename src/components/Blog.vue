<script setup>
import { defineEmits } from 'vue';
import axios from 'axios';

const emit = defineEmits();

defineProps({
	id: String,
	title: String,
	content: String,
	tag: String
});

const handleDelete = async (id) => {
	try {
		await axios.delete(`https://localhost:8000/api/articles/delete/${id}`)
		emit('blog-deleted', id);
	} catch (error) {
		console.error('Error while deleting blog', error);
	}
}
</script>

<template>
	<div class="blog">
		<h2>{{ title }}</h2>
		<span @click="handleDelete(id)" class="deleteSpan">&#10005;</span>
		<span class="editSpan">Edit</span>
		<p class="tag">{{ tag }}</p>
		<p class="content">{{ content }}</p>
		<p class="author">Mateo Novosel</p>
	</div>
</template>

<style scoped>
	.blog {
		margin-top: 60px;
		box-shadow: 1px 1px 10px black;
		width: 600px;
		height: auto;
		padding: 20px 10px 10px 20px;
	}
	
	.editSpan {
		float: right;
		padding-right: 15px;
		padding-top: 3px;
		text-decoration: underline;
		color: blue;
	}
	
	.editSpan:hover {
		cursor: pointer;
	}
	
	.deleteSpan {
		float: right;
		padding-right: 15px;
	}
	
	.deleteSpan:hover {
		cursor: pointer;
	}
	
	h2 {
		display: inline;
		font-size: 26px;
		margin: 0;
		width: fit-content;
	}
	
	.author {
		float: right;
		font-size: 17px;
		margin: 0px;
		margin-right: 7px;
	}
	
	.tag {
		margin: 0;
		font-size: 20px;
	}
	
	.content {
		font-size: 16px;
	}
</style>