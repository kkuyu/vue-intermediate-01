<template>
	<div id="app">
		<TodoHeader />
		<TodoInput v-on:addTodoItem="addOneItem" />
		<TodoList
			v-bind:propsdata="todoItems"
			v-on:removeTodoItem="removeOneItem"
			v-on:toggleItem="toggleOneItem"
		/>
		<TodoFooter v-on:clearAll="clearAllItem" />
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
			var date = new Date();
			var obj = {
				time: this.getDateStr(date),
				timestamp: date.getTime(),
				completed: false,
				item: todoItem,
			};
			localStorage.setItem("todo_" + obj.timestamp, JSON.stringify(obj));
			this.todoItems.push(obj);
		},
		removeOneItem: function(todoItem, index) {
			localStorage.removeItem("todo_" + todoItem.timestamp);
			this.todoItems.splice(index, 1);
		},
		toggleOneItem: function(todoItem, index) {
			this.todoItems[index].completed = !this.todoItems[index].completed;
			localStorage.removeItem("todo_" + todoItem.timestamp);
			localStorage.setItem(
				"todo_" + todoItem.timestamp,
				JSON.stringify(todoItem)
			);
		},
		clearAllItem: function() {
			localStorage.clear();
			this.todoItems = [];
		},
		getDateStr: function(date) {
			return `Date:
        ${date.getDate()}/${date.getMonth() + 1}/${date.getFullYear()}
				${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`;
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
