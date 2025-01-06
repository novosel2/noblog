<script setup>
defineProps({
	title: String,
	content: String,
	tag: String
});
</script>

<template>
	<div class="blog">
		<h2>{{ title }}</h2>
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