<template>
	<h1>ToDo App</h1>
	<form @submit.prevent="addTodo()">
		<label>New ToDo </label>
		<input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
		>
		<button>Add ToDo</button>
	</form>
	<h2>ToDo List</h2>
	<ul>
		<li
			v-for="(todo, index) in todos"
			:key="index"
		>
			<span
				:class="{ done: todo.done }"
				@click="doneTodo(todo)"
			>{{ todo.content }}</span>
			<button @click="removeTodo(index)">Remove</button>
		</li>
	</ul>
	<h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script>
	import { ref } from 'vue';
	export default {
		name: 'App',
		setup () {
			const newTodo = ref('');
			const defaultData = [{
				done: false,
			}]
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
			const todos = ref(todosData);
			function addTodo () {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				saveData();
			}

			function doneTodo (todo) {
				todo.done = !todo.done
				saveData();
			}

			function removeTodo (index) {
				todos.value.splice(index, 1);
				saveData();
			}

			function saveData () {
				const storageData = JSON.stringify(todos.value);
				localStorage.setItem('todos', storageData);
			}

			return {
				todos,
				newTodo,
				addTodo,
				doneTodo,
				removeTodo,
				saveData
			}
		}
	}
</script>

<style lang="scss">
body {
    margin: 0;
    padding: 0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    background-color: #f6f6f6;
    color: кwhite;
}

#app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
}

#app h1 {
    font-weight: bold;
    font-size: 28px;
    text-align: center;
}

#app form {
    display: flex;
    flex-direction: column;
    width: 100%;
}

#app form label {
    font-size: 14px;
    font-weight: bold;
}

#app form input,
#app form button {
    height: 48px;
    box-shadow: none;
    outline: none;
    padding-left: 12px;
    padding-right: 12px;
    border-radius: 6px;
    font-size: 18px;
    margin-top: 6px;
    margin-bottom: 12px;
}

#app form input {
    background-color: transparent;
    border: 2px solid rgb(0, 0, 0);
    color: inherit;
}

#app form button {
    cursor: pointer;
    background-color: #4b5fe1;
    border: 1px solid #a0a4d9;
    color: #1f2023;
    font-weight: bold;
    outline: none;
    border-radius: 6px;
}

#app h2 {
    font-size: 22px;
    border-bottom: 2px solid rgb(0, 0, 0);
    padding-bottom: 6px;
}

#app ul {
    padding: 10px;
}

#app ul li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 2px solid rgb(0, 0, 0);
    padding: 12px 24px;
    border-radius: 6px;
    margin-bottom: 12px;
}

#app ul li span {
    cursor: pointer;
}

#app ul li span.done {
    text-decoration: line-through;
}

#app ul li button {
    font-size: 12px;
    padding: 6px;
    background: red;
}

#app h4 {
    text-align: center;
    opacity: 0.5;
    margin: 0;
}

</style>