<script>
	import { onMount } from 'svelte';
	const ENTER_KEY = 13;
	const ESCAPE_KEY = 27;

	let currentFilter = 'all';
	let newTodo = '';
	let todos = [
		{
			id: 1,
			completed: false,
			title: 'Go to store',
			editing: false,
		},
		{
			id: 2,
			completed: false,
			title: 'Workout for 30 mins',
			editing: false,
		},
		{
			id: 3,
			completed: false,
			title: 'Take over the nearest Jack in the Box',
			editing: false,
		}
	]

	function addTodo(event){
		if(event.which === ENTER_KEY){

			todos = [...todos, {
				id: todos.length + 1,
				completed: false,
				title: newTodo,
				editing: false
			}]

			todos = todos;
			newTodo = '';
		}

	}

	function editTodo(todo){
		todo.editing = true;
		todos = todos;
	}

	function doneEdit(todo){
		todo.editing = false;
		todos = todos;
	}
	
	function doneEditKeyDown(todo, event){
		if(event.which === ENTER_KEY){
			doneEdit(todo);
		}
	}

	function deleteTodo(todoId){
		todos = todos.filter( todo => todo.id != todoId);
	}

	function checkAllTodos(event){
		todos = todos.map(todo => ({...todo, completed: event.target.checked}));
	}
	
	function clearCompleted(){
		todos = todos.filter(todo => !todo.completed);
	}

	function updateFilter(filter){
		currentFilter = filter;
	}

	onMount(async () => {
		const res = await fetch("https://api.kanye.rest");
		const response = await res.json();
		console.log(response.quote);
	});

	$: filteredTodos = currentFilter === 'all' 
		? todos 
		: currentFilter === 'completed' 
			? todos.filter(todo => todo.completed) 
			: todos.filter(todo => !todo.completed)
	$: todosRemaining = todos.filter(todo => !todo.completed).length;
</script>

<style lang="scss">
	.container {
		max-width: 600px;
		margin: 0 auto;
	}
	.logo {
		display: block;
		margin: 20px auto;
		height: 75px;
	}
	.todo-input{ 
		width: 100%;
		padding: 10px 18px;
		font-size: 18px;
		margin-bottom: 16px;
	}
	.todo-item {
		margin-bottom: 12px;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.remove-item {
		cursor: pointer;
		margin-left: 14px;
		&:hover {
			color: black;
		}
	}
	.todo-item-left {
		display: flex;
		align-items: center;
	}
	.todo-item-label {
		padding: 10px;
		border: 1px solid white;
		margin-left: 12px;
	}
	.todo-item-edit {
		font-size: 24px;
		color: #2c3e50;
		margin-left: 12px;
		width: 100%;
		padding: 10px;
		border: 1px solid #ccc;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		&:focus {
			outline: none;
		}
	}
	.completed {
		text-decoration: line-through;
		color: grey;
	}
	.extra-container {
		display: flex;
		align-items: center;
		justify-content: space-between;
		font-size: 16px;
		border-top: 1px solid lightgrey;
		margin-bottom: 14px;

		input {
			margin-right: 8px;
		}
	}
	button {
		font-size: 14px;
		background-color: white;
		appearance: none;
		&:hover {
			background: salmon;
		}
		&:focus {
			outline: none;
		}
	}
	.active {
		background: salmon;
	}
</style>

<div class="container">
	<!-- <img src={''} alt="svelte logo" class="logo"/> -->
	
	<input bind:value={newTodo} type="text" class="todo-input" placeholder="What needs to be done" 
	on:keydown={addTodo}/>
	{#each filteredTodos as todo, i }
		<div class="todo-item">
			<div class="todo-item-left">
				<input type="checkbox" bind:checked={todo.completed}>
				{#if !todo.editing}
					<div class="todo-item-label" class:completed={todo.completed} on:dblclick={() => editTodo(todo)}>{todo.title}</div>
				{:else}
					<input class="todo-item-edit" bind:value={todo.title} type="text" 
					on:blur={() => doneEdit(todo)} 
					on:keydown={() => doneEditKeyDown(todo, event)} autofocus>
				{/if}
			</div>
			<div class="remove-item" on:click={() => deleteTodo(todo.id)}>
				&times;
			</div>
		</div>
	{/each}
	<div class="extra-container">
		<div>
			<label>
				<input type="checkbox" on:change={checkAllTodos}>Check All
			</label>
		</div>
		<div>
			{todosRemaining} items left
		</div>
	</div>
	
	<div class="extra-container">
		<div>
			<button on:click={()=> updateFilter('all')}  class:active="{currentFilter === "all"}">All</button>
			<button on:click={()=> updateFilter('active')} class:active="{currentFilter === "active"}">Active</button>
			<button on:click={()=> updateFilter('completed')} class:active="{currentFilter === "completed"}">Completed</button>
		</div>

		<div>
			<button on:click={clearCompleted}>Clear Completed</button>
		</div>
	</div>
</div>
