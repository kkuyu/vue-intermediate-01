<template>
	<div id="app">
		<TodoHeader />
		<TodoInput v-on:addTodoItem="addOneItem" />
		<TodoList v-bind:propsdata="todoItems" />
		<TodoFooter />
	</div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";
import TodoFooter from "./components/TodoFooter.vue";

export default {
	data: function() {
		return { todoItems: [] };
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
			this.sortList();
		}
	},
	methods: {
		addOneItem: function(todoItem) {
			var obj = {
				timestamp: new Date().getTime(),
				completed: false,
				item: todoItem,
			};
			localStorage.setItem("todo_" + todoItem, JSON.stringify(obj));
			this.todoItems.push(obj);
		},
		sortList: function() {
			this.todoItems.sort((a, b) => {
				return a.timestamp - b.timestamp;
			});
		},
	},
	components: {
		TodoHeader: TodoHeader,
		TodoInput: TodoInput,
		TodoList: TodoList,
		TodoFooter: TodoFooter,
	},
};
</script>

<style>
body {
	text-align: center;
	background-color: #f6f6f6;
}
input {
	border-style: groove;
	width: 200px;
}
button {
	border-style: groove;
}
.shadow {
	box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
