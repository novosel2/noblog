<script setup>
defineProps({
	isVisible: true,
	blog: Object
});

const closeModal = () => {
	isVisible = false;
}
</script>

<template>
	<div v-if="isVisible" class="popout">
    <div class="popout-content">
      <button @click="closeModal" class="close-btn">X</button>
      <h3>Edit Post</h3>
			<input :value="blog.title"/>
      <textarea v-model="postContent" placeholder="Edit your post here..."></textarea>
      <button @click="savePost">Save</button>
    </div>
  </div>
</template>

<style scoped>
	.popout {
		position: fixed;
		top: 0;
		right: 0;
		left: 0;
		bottom: 0;
		background-color: rgb(255, 255, 255);
		display: flex;
		justify-content: center;
		align-items: center;
		margin: auto;
		box-shadow: 1px 1px 10px black;
		
		
		width: 500px;
		height: 500px;
	}
	
	.popout-content {
		width: 100%;
		height: 100%;
	}
	
	.close-btn {
		
	}
</style>