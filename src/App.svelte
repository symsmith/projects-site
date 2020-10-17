<script>
	let currentValue = '';
	let tasks = [];

	function toggleCheckByAttr(arr, attr, value) {
		let i = arr.length;
		while(i--){
			if( arr[i] 
				&& arr[i].hasOwnProperty(attr) 
				&& (arguments.length > 2 && arr[i][attr] === value ) ) {
				    arr[i].checked = !arr[i].checked;
			}
		}
		return arr;
	}
	function addTask(e) {
		if (currentValue !== '') {
			let n = tasks.length;
			let lastId = n === 0 ? -1 : tasks[0].id;
			tasks.unshift({id: lastId + 1, value: currentValue, checked: false});
			tasks = tasks;
			currentValue = '';
		}
	}
	function checkTask(i) {
		toggleCheckByAttr(tasks, 'id', i);
		tasks = tasks;
	}
</script>

<main>
	<h1>To-do</h1>
	<form>
		<input required type="text" name="newtask" id="newtask" placeholder="New Task" bind:value={currentValue}>

		<button type="submit" on:click|preventDefault={addTask}>Add</button>
	</form>

	<h2>Tasks:</h2>
	{#if tasks.length === 0}
		<p>No task yet…</p>
	{/if}
	<ul>
	{#each tasks as {id, value, checked}}
		<li>
			<span class="check" on:click={checkTask(id)}>{checked ? "☑" : ""}</span> 
			<span style={checked ? "text-decoration: line-through" : ""}>{value}</span>
		</li> 
	{/each}
	</ul>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	ul {
		width: 300px;
		margin: auto;
		text-align: left;
		list-style-type: none;
		overflow-wrap: break-word;
	}

	.check {
		cursor: pointer;
		font-family: "todo-font";
		margin: 3px;
		position: relative;
		right: 5px;
	}

	@font-face {
		font-family: "todo-font";
		src: url("/font/todo-font.ttf") format("ttf"),
			 url("/font/todo-font.woff") format("woff"),
			 url("/font/todo-font.eot") format("eot"),
			 url("/font/todo-font.svg") format("svg"),
			 url("/font/todo-font.woff2") format("woff2");
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
			width: 100%;
			margin: none;
			padding: none;
		}
	}
</style>