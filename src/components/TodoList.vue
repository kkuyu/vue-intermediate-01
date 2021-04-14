<template>
	<div>
		<ul>
			<li
				v-for="(todoItem, index) in todoItems"
				v-bind:key="index + todoItem.item"
				class="shadow"
			>
				<i
					class="fas fa-check checkBtn"
					v-bind:class="{ checkBtnCompleted: todoItem.completed }"
					v-on:click="toggleComplete(todoItem, index)"
				/>
				<span v-bind:class="{ textCompleted: todoItem.completed }">{{
					todoItem.item
				}}</span>
				<span class="removeBtn" v-on:click="() => removeTodo(todoItem, index)">
					<i class="fas fa-trash-alt " />
				</span>
				<!-- <button v-on:click=""></button> -->
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	data: function() {
		return {
			todoItems: [],
		};
	},
	methods: {
		removeTodo: function(todoItem, index) {
			console.log("removeTodo", todoItem, index);
			localStorage.removeItem("todo_" + todoItem.item);
			this.todoItems.splice(index, 1);
		},
		toggleComplete: function(todoItem) {
			todoItem.completed = !todoItem.completed;
			localStorage.removeItem("todo_" + todoItem.item);
			localStorage.setItem(
				"todo_" + this.newTodoItem,
				JSON.stringify(todoItem)
			);
		},
	},
	created: function() {
		console.log("created");
		if (localStorage.length > 0) {
			for (var i = 0; i < localStorage.length; i++) {
				if (localStorage.key(i).indexOf("todo_") === -1) continue;
				this.todoItems.push(
					JSON.parse(localStorage.getItem(localStorage.key(i)))
				);
			}
		}
	},
};
</script>

<style scoped>
ul {
	list-style-type: none;
	padding-left: 0;
	margin-top: 0;
	text-align: left;
}
li {
	display: flex;
	min-height: 50px;
	height: 50px;
	line-height: 50px;
	margin: 0.5rem 0;
	padding: 0 0.9rem;
	background: #fff;
	border-radius: 5px;
}
.removeBtn {
	margin-left: auto;
	color: #de4343;
}
.checkBtn {
	line-height: 45px;
	color: #62acde;
	margin-right: 5px;
}
.checkBtnCompleted {
	color: #b3adad;
}
.textCompleted {
	text-decoration: line-through;
	color: #b3adad;
}
</style>
