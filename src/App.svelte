<script>

	import dayjs from 'dayjs';
	import relativeTime from 'dayjs/plugin/relativeTime'
	import {v4} from 'uuid';
	import md5 from 'md5';
	import Background from './Background.svelte';
	import Darken from './Darken.svelte';
	import Music from './Music.svelte';
	import colorContrast from 'color-contrast';
	import Credits from './Credits.svelte';

	dayjs.extend(relativeTime);

	let greetingString = new Date().getHours() < 12 ? 'Good morning' : (new Date().getHours() < 17 ? 'Good afternoon' : (new Date().getHours() < 20 ? 'Good evening' : 'Good night'));

	let timeString = `It is currently ${dayjs().format('h:mma')}`;
	let descriptionValue = '';

	let bgColor = 'FAFAFA';
	let fgColor = '333';
	let doColorChanges = false;

	let todos = [];

	const currentISO = new Date().toISOString();

	let datetimeValue = "";
	let textValue = "";

	const syncToLs = t => {
		localStorage.setItem("todos", JSON.stringify(t))
	}
	
	const getLs = d => {
		return JSON.parse(localStorage.getItem("todos") || JSON.stringify(d))
	}

	const addTodo = e => {
		if (e.key === 'Enter' && textValue.trim()) {
			todos = [
				...todos,
				{
					text: textValue,
					id: v4(),
					due: datetimeValue ? new Date(datetimeValue) : null,
					description: descriptionValue.length > 0 ? descriptionValue : null
				}
			]

			textValue = "";
			descriptionValue = "";
		}

		if (e.key === 'Backspace' && textValue.length === 0 && e.target.dataset.main === '1') {

			const last = todos.pop();
			if (!last) return;
			todos = todos.filter(todo => todo.id !== last.id);
			textValue = last.text;

			datetimeValue = last.due.toISOString().substring(0,last.due.toISOString().length-1);
			descriptionValue = last.description || "";

		}

		syncToLs(todos);

	}

	const removeTodo = id => {
		todos = todos.filter(todo => todo.id !== id);
		syncToLs(todos);
	}

	todos = getLs(todos);

	setInterval(() => {

		timeString = `It is currently ${dayjs().format('h:mma')}`;

		if (doColorChanges) {
			bgColor = md5(timeString).substring(0,6);
			fgColor = colorContrast("#333", bgColor) > 4.5 ? "333" : "FFFFFF";
		} else {
			bgColor = "FAFAFA";
			fgColor = "333";
		}

		greetingString = new Date().getHours() < 12 ? 'Good morning' : (new Date().getHours() < 17 ? 'Good afternoon' : (new Date().getHours() < 20 ? 'Good evening' : 'Good night'));

	}, 1000);

</script>

<Credits />

<main style="background-color: #{bgColor}; color: #{fgColor}">

	<h1>{greetingString}</h1>
	<span class="time"><i>{timeString}</i></span>

	<h2>To-do list</h2>

	{#each todos as todo}
		<div title={todo.id} class="todo" style="border: 2px solid #{md5(todo.text).substring(0,6)}">

			<span class="text">{todo.text} <i>- {todo.due ? dayjs().to(todo.due) : "no due date"}</i></span>
			<span class="remove" on:click={() => removeTodo(todo.id)}>&#x274C;</span>

			{#if todo.description}
				<span class="description">{todo.description}</span>
			{/if}

		</div>
	{/each}

	{#if todos.length === 0}
		<div class="todo">
			<span class="text">Nothing here yet</span>
			<span class="description">Add a to-do list item using the form below. It will be saved in your local storage.</span>
		</div>
	{/if}

	<div class="new-todo">

		<h5 style="width:100%; margin: 0; margin-left: 5px; margin-bottom: 5px">add a todo (press enter when done)</h5>

		<input
			type='text'
			data-main="1"
			on:keydown={addTodo}
			bind:value={textValue}	
			placeholder="Todo text"
		/> <input type="date" on:keydown={addTodo} bind:value={datetimeValue} />

		<input
			class="description"
			type="text"
			placeholder="Description (optional)"
			bind:value={descriptionValue}
			on:keydown={addTodo}
		/>

	</div>

	<!-- <div style="margin-top: 10px">
		<label>
			<input type="checkbox" bind:checked={doColorChanges} /> Do color changes
		</label>
	</div> -->
	

</main>

<Background />
<Darken />
<Music />

<style>

	h1 {
		line-height: 1em;
		margin-bottom: 15px;
		font-size: 4em;
		vertical-align: middle;
	}

	main {

		padding: 25px;
		border-radius: 5px;
		box-shadow: 10px 10px 103px -50px rgba(0,0,0,0.75);

		transition: 0.5s;
	}

	.todo {
		padding: 10px;
		border: 2px solid #EEE;
		margin: 5px 0;
		border-radius: 3px;
		transition: 0.25s;

		display: flex;
		flex-flow: row wrap;
		justify-content: space-between;
		align-items: center;
	}

	.todo:hover {
		background-color: #F5F5F5;
	}
	
	.todo:active {
		border: 2px solid gray;
	}

	.remove {
		color: red;
		vertical-align: middle;
		font-size: 0.8rem;
		cursor: pointer;
		user-select: none;
	}

	.time {
		font-size: 1.5em;
	}

	.new-todo {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		flex-wrap: wrap;
		padding: 10px;
		border: 2px dashed #EEE;
		margin-top: 1em;
	}

	.new-todo input {
		flex-grow: 1;
		margin: 5px;
		padding: 5px;
		font-size: 0.8em;
		border: 2px solid #EEE;
		padding: 8px;
	}

	input.description {
		width: 100%;
	}
	
	.todo .description {
		width: 100%;
		/* color: gray; */
		font-size: 0.8rem;
	}

	input {
        background-color: transparent;
		outline: none;
    }

	::placeholder {
		color: inherit;
		opacity: 0.5;
	}

</style>