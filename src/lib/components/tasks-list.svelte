<script lang="ts">
	import type { Task } from '$lib/model/todo'
	import { fade } from 'svelte/transition'

	let {
		tasks,
		toggleDone,
		removeTask,
	}: {
		tasks: Task[]
		toggleDone: (task: Task) => void
		removeTask: (id: string) => void
	} = $props()
</script>

<section>
	{#each tasks as task (task.id)}
		<article class="task" transition:fade>
			<label>
				<input checked={task.done} onchange={() => toggleDone(task)} type="checkbox" />
				<span class:done={task.done}>{task.title}</span>
			</label>
			<button onclick={() => removeTask(task.id)}>Remove</button>
		</article>
	{/each}
</section>

<style>
	section {
		max-height: 420px;
		overflow-y: auto;
	}
	label {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 20px;
		color: #000;
		font-size: 20px;
		cursor: pointer;
		user-select: none;
	}
	input {
		width: 20px;
		height: 20px;
	}
	button {
		background-color: #0172ad;
		color: #fff;
		border: none;
		padding: 16px 20px;
	}
	.done {
		text-decoration: line-through;
	}

	.task {
		padding: 20px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		background-color: #fff;
		border-radius: 8px;
	}
	.task + .task {
		margin-top: 10px;
	}
</style>
