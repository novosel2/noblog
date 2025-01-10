<script setup>
import { reactive } from 'vue';

const emit = defineEmits();

const props = defineProps({
	blog: Object
});

const state = reactive({
	title: props.blog.title,
	content: props.blog.content,
	tag: props.blog.tag
})

const closePopout = () => {
	document.body.style.paddingRight = '';
	document.body.classList.remove('no-scroll');
	emit('close-popout');
}

const savePost = () => {
	emit('save-blog', state);
}
</script>

<template>
	<div class="overlay">
		<div class="popout">
			<div class="popout-content">
				<button @click="closePopout" class="close-btn">X</button>
				<h3>Edit Post</h3>
				<div class="form-group">
					<label>Title: </label>
					<input v-model="state.title" required/>
				</div>
				<div class="form-group">
					<label>Tag: </label>
					<select v-model="state.tag" required>
						<option>General</option>
						<option>Work</option>
						<option>Hobby</option>
						<option>Food</option>
						<option>Entertainment</option>
					</select>
				</div>
				<textarea v-model="state.content" class="text-area" required>{{ state.content }}</textarea>
				<button @click="savePost" class="save-btn">Save</button>
			</div>
		</div>
	</div>
</template>

<style scoped>
	.overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.7);
		z-index: 1000;
	}

	.popout {
		position: fixed;
		top: 0;
		right: 0;
		left: 0;
		bottom: 0;
		background-color: #363636;
		display: flex;
		justify-content: center;
		align-items: center;
		margin: auto;
		box-shadow: 1px 1px 10px #707070;
		
		
		width: 500px;
		height: 500px;
	}
	
	.popout-content {
		width: 80%;
		height: 80%;
		display: flex;
		flex-direction: column;
	}
	
	.close-btn {
		width: 30px;
		align-self: flex-end;
		border: 2px black outset;
		color: white;
		background-color: #444;
	}
	
	.save-btn {
		width: 80px;
		margin-top: 10px;
		align-self: flex-end;
		border: 2px black outset;
		color: white;
		background-color: #444;
	}
	
	.form-group {
		display: flex;
		align-items: center;
	}
	
	.form-group label {
		padding-bottom: 12px;
	}
	
	button:hover {
		cursor: pointer;
	}

	button:active {
		border: 2px black inset;
	}
</style>