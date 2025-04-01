<script lang="ts">
	let newTask = $state('')
	let {
		addTask,
	}: {
		addTask: (newTask: string) => void
	} = $props()

	function formSubmitted(e: SubmitEvent) {
		e.preventDefault()
		if (!newTask) return
		addTask(newTask)
		newTask = ''
	}
</script>

<form onsubmit={formSubmitted}>
	<label for="new-task">Task name</label>
	<input id="new-task" name="newTask" bind:value={newTask} />
	<div class="button-container">
		<button>Add</button>
	</div>
</form>

<style>
	form {
		margin: 30px 0;
		display: flex;
		flex-direction: column;
		gap: 16px;
	}
	input {
		padding: 20px;
		color: #333;
	}
	.button-container {
		display: flex;
		justify-content: end;
	}
	button {
		background-color: #0172ad;
		color: #fff;
		border: none;
		padding: 16px 20px;
	}
</style>
