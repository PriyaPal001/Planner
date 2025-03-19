
<script lang='ts'>
    import { Button } from "$lib/components/ui/button/index.js";
    import { Input } from "$lib/components/ui/input/index.js";
    import { Checkbox } from "$lib/components/ui/checkbox/index.js";


	let newTodo=$state('');
    let err=$state('');

	let todoList =$state([
		{
			id:1,
			text:'Learn Svelte',
			completed:false,
            
		}
	])

	function addtodo(){
        todoList.push({
            id:todoList.length+1,
            text:newTodo,
            completed:false,
        })
		newTodo='';
	}

	function remove(id:number){
        
		todoList=todoList.filter((item)=>item.id!=id);

	}
</script>



<main class="container mx-auto p-4 content-center justify-center ">

<h1 class="text-primary text-2xl m-10">Study Planner</h1>

<form action="">
    {#if err}
        <p class="text-red-500">{err}</p>
    {/if}
	<Input type="text" bind:value={newTodo} placeholder="Add a new topic" />
	<Button type="submit" onclick={()=>{
        if(newTodo==''){
            err='Please enter a topic';
        }
        else{
            addtodo();
            err='';
        }
    }} >Add</Button>
</form>

{#each todoList as item}
	<div>
		<Checkbox  bind:checked={item.completed} class=""/>
		<span class:checked={item.completed} class="">{item.text}</span>
		<button onclick={()=>{
			remove(item.id)
		}}>❌</button>	
	</div>
{/each}

    
</main>

<style>
	.checked{
		text-decoration:line-through;
	}
</style>