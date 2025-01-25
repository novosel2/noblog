<script setup>
import { defineEmits, ref } from 'vue';
import axios from 'axios';
import PopoutBlog from './PopoutBlog.vue';

const emit = defineEmits();

const props = defineProps({
	blog: Object
});

var isEditActive = ref(false);

const options = { month: 'short', day: 'numeric', year: 'numeric' };
const date = new Date(props.blog.dateCreated);
const formattedDate = date.toLocaleDateString('en-US', options);

const handleDelete = async () => {
	try {
		await axios.delete(`https://localhost:8000/api/articles/delete/${props.blog.id}`)
		emit('blog-deleted', props.blog.id);
	} catch (error) {
		console.error('Error while deleting blog', error);
	}
}

const updateBlog = async (updatedBlog) => {
	try {
		await axios.put(`https://localhost:8000/api/articles/update-article/${props.blog.id}`, updatedBlog);
		emit('blog-updated', props.blog.id, updatedBlog);
		console.log(updatedBlog);
	} catch (error) {
		console.error('Error while updating blog', error);
	} finally {
		isEditActive.value = false;
	}
}

const showEdit = () => {
	isEditActive.value = !isEditActive.value;
	if (isEditActive.value) {
		document.body.style.paddingRight = `${window.innerWidth -  document.documentElement.clientWidth}px`;
		document.body.classList.add('no-scroll');
	}
};
</script>

<template>
	<div class="blog">
		<h2>{{ blog.title }}</h2>
		<span @click="handleDelete(blog.id)" class="deleteSpan">&#10005;</span>
		<span @click="showEdit" class="editSpan">Edit</span>
		<p class="tag">{{ blog.tag }}</p>
		<p class="content" v-html="blog.content"></p>
		<div class="blog-footer">
			<p class="date">{{ formattedDate }}</p>
			<p class="author">Mateo Novosel</p>
		</div>
		<PopoutBlog v-if="isEditActive" @close-popout="showEdit" @save-blog="updateBlog" :blog="blog"/>
	</div>
</template>

<style scoped>
	.blog {
		margin-top: 60px;
		box-shadow: 1px 1px 10px #707070;
		width: 600px;
		height: auto;
		padding: 25px 10px 10px 20px;
		background-color: #363636;
		color: white;
	}
	
	.blog-footer {
		display: flex;
		justify-content: space-between;
	}
	
	.author, .date {
		font-size: 17px;
		color: #8e8e8e;
		margin: 0px;
		margin-top: 10px;
	}
	
	.author {
		margin-right: 10px;
	}
	
	.editSpan {
		float: right;
		padding-right: 15px;
		padding-top: 3px;
		text-decoration: underline;
		color: yellow;
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
	
	.tag {
		margin: 0;
		font-size: 20px;
		color: #8e8e8e;
	}
	
	.content {
		font-size: 16px;
		line-height: 24px;
		white-space: pre-line;
	}
</style>