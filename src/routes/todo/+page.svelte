<script lang="ts">
	import TasksForm from '$lib/components/tasks-form.svelte'
	import TasksList from '$lib/components/tasks-list.svelte'
	import type { Filter, Task } from '$lib/model/todo'

	let title = 'Tasks App'
	let currentFilter = $state<Filter>('all')
	let tasks = $state<Task[]>([])
	let totalDone = $derived(tasks.reduce((total, task) => total + Number(task.done), 0))

	let filteredTasks = $derived.by(() => {
		switch (currentFilter) {
			case 'all': {
				return tasks
			}
			case 'done': {
				return tasks.filter((task) => task.done)
			}
			case 'todo': {
				return tasks.filter((task) => !task.done)
			}
			default:
				return tasks
		}
	})

	function addTask(newTask: string) {
		tasks.push({
			id: crypto.randomUUID(),
			title: newTask,
			done: false,
		})
	}

	function toggleDone(task: Task) {
		task.done = !task.done
	}

	function removeTask(id: string) {
		const index = tasks.findIndex((task) => task.id === id)
		tasks.splice(index, 1)
	}
</script>

{#snippet filterButton(filter: Filter)}
	<button
		onclick={() => (currentFilter = filter)}
		class:contrast={currentFilter === filter}
		class="secondary filterButton">{filter}</button
	>
{/snippet}

<!-- <svelte:head>
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css" />
</svelte:head> -->

<h1>{title}</h1>
<TasksForm {addTask} />
<p>
	{#if tasks.length}
		{totalDone} / {tasks.length} tasks completed
	{:else}
		Add a task to get started.
	{/if}
</p>
{#if tasks.length}
	<div class="button-container">
		{@render filterButton('all')}
		{@render filterButton('todo')}
		{@render filterButton('done')}
	</div>
{/if}
<TasksList tasks={filteredTasks} {toggleDone} {removeTask} />

<style>
	.button-container {
		display: flex;
		justify-content: end;
		margin-bottom: 1rem;
		gap: 10px;
	}

	.filterButton {
		text-transform: capitalize;
		padding: 16px 20px;
	}
	button.secondary {
		background-color: #757575;
		border: none;
	}
	button.secondary.contrast {
		background-color: #252525;
	}
</style>
