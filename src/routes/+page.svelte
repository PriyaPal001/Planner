<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import { Input } from '$lib/components/ui/input/index.js';
	import { Checkbox } from '$lib/components/ui/checkbox/index.js';
	import { dndzone } from 'svelte-dnd-action';

	let newTodo = $state('');
	let err = $state('');

	let todoList = $state([
		{
			id: "svelte",
			text: 'Learn Svelte',
			completed: false
		}
	]);

	function addtodo() {
		todoList.push({
			id: crypto.randomUUID(),
			text: newTodo,
			completed: false
		});
		newTodo = '';
	}

	function remove(id: string) {
		todoList = todoList.filter((item) => item.id != id);
	}

	const flipDurationMs = 300;
	function handleDndConsider(e) {
		todoList = e.detail.items;
	}
	function handleDndFinalize(e) {
		todoList = e.detail.items;
	}
</script>

<main class="container mx-auto content-center justify-center p-4">
	<h1 class="m-10 text-2xl text-primary">Study Planner</h1>

	<form action="">
		{#if err}
			<p class="text-red-500">{err}</p>
		{/if}
		<Input type="text" bind:value={newTodo} placeholder="Add a new topic" />
		<Button
			type="submit"
			onclick={() => {
				if (newTodo == '') {
					err = 'Please enter a topic';
				} else {
					addtodo();
					err = '';
				}
			}}>Add</Button
		>
	</form>

	<div
		class="h-60"
		use:dndzone={{ items: todoList, flipDurationMs }}
		onconsider={handleDndConsider}
		onfinalize={handleDndFinalize}
	>
		{#each todoList as item (item.id)}
			<div>
				<Checkbox bind:checked={item.completed} class="" />
				<span class:checked={item.completed} class="">{item.text}</span>
				<button
					onclick={() => {
						remove(item.id);
					}}>❌</button
				>
			</div>
		{/each}
	</div>
</main>

<style>
	.checked {
		text-decoration: line-through;
	}
</style>
