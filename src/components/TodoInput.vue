<template>
	<div class="inputBox shadow">
		<input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo" />
		<span class="addContainer" v-on:click="addTodo">
			<i class="fas fa-plus addBtn" />
		</span>

		<!-- use the modal component, pass in the prop -->
		<Modal v-if="showModal" @close="showModal = false">
			<h3 slot="header">경고!</h3>
			<p slot="body">
				내용을 입력하세요
			</p>
			<div slot="footer">
				<button class="closeModalBtn" @click="showModal = false">
					<i class="fas fa-times"></i>
				</button>
			</div>
		</Modal>
	</div>
</template>

<script>
import Modal from "./common/Modal";

export default {
	data() {
		return {
			newTodoItem: "",
			showModal: false,
		};
	},
	methods: {
		addTodo() {
			if (this.newTodoItem !== "") {
				this.$emit("addTodoItem", this.newTodoItem);
				this.clearInput();
			} else {
				this.showModal = true;
			}
		},
		clearInput() {
			this.newTodoItem = "";
		},
	},
	components: {
		Modal,
	},
};
</script>

<style scoped>
input:focus {
	outline: none;
}
.inputBox {
	background: #fff;
	height: 50px;
	line-height: 50px;
	border-radius: 5px;
}
.inputBox input {
	border-style: none;
	font-size: 0.9rem;
}
.addContainer {
	float: right;
	background: linear-gradient(to right, #6478fb, #8763fb);
	display: block;
	width: 3rem;
	border-radius: 0 5px 5px 0;
}
.addBtn {
	color: white;
	vertical-align: middle;
}
.closeModalBtn {
	position: absolute;
	top: 15px;
	right: 15px;
	color: #42b983;
}
</style>
