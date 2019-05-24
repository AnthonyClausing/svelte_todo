<script>
	const ENTER_KEY = 13;
	const ESCAPE_KEY = 27;

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
	function deleteTodo(todoId){
		todos = todos.filter( todo => todo.id != todoId)
	}
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
	{#each todos as todo, i }
		<div class="todo-item">
			<div class="todo-item-left">
				<input type="checkbox" >
				<div class="todo-item-label">{todo.title}</div>
			</div>
			<div class="remove-item" on:click={() => deleteTodo(todo.id)}>
				&times;
			</div>
		</div>
	{/each}
	<div class="extra-container">
		<div>
			<label>
				<input type="checkbox">Check All
			</label>
		</div>
		<div>
			3 items left
		</div>
	</div>
	
	<div class="extra-container">
		<div>
			<button>All</button>
			<button>Active</button>
			<button>Completed</button>
		</div>

		<div>
			<button>Clear Completed</button>
		</div>
	</div>
</div>
