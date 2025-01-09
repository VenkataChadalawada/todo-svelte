<script lang="ts">
    import {TasksForm, TaskList} from '../components/';
    import type { Task, Filter } from '../lib/types';

    let message = "Tasks App";
    let currentFilter = $state<Filter>('all');
    let tasks = $state<Task[]>([]);
    let totalDone = $derived(tasks.reduce((total, task) => total+Number(task.done), 0));
    function addTask(newTask: string){
        console.log("--addtask--", newTask);
        tasks.push({
            id: crypto.randomUUID(),
            title: newTask,
            done: false
        });
    }
    let filteredtasks = $derived.by(() => {
        switch(currentFilter){
            case 'all':
                return tasks;       
            case 'done':
                return tasks.filter((task) => task.done);
            case 'todo':
                return tasks.filter((task) => !task.done);    
        }
        return tasks;
    })
    function toggleDone(task: Task) {
        task.done = !task.done;
    }
    function removeTask(id: string){
        const index = tasks.findIndex((task => task.id === id));
        tasks.splice(index, 1);
    }
</script>

{#snippet filterButton(filter: Filter)}
    <button onclick={() => currentFilter=filter} class:contrast={currentFilter===filter} class="secondary filterButton"> {filter} </button>            
{/snippet}

<main>
    <h1>{message}</h1>
    <TasksForm {addTask}/>
    <p> 
        {#if tasks.length}
            {totalDone}/{tasks.length} tasks completed
        {:else}
            Add a task to get started
        {/if}
    </p>
    {#if tasks.length}
        <div class="button-container">
            {@render filterButton('all')}
            {@render filterButton('todo')}
            {@render filterButton('done')}
        </div>
    {/if}
    <TaskList tasks={filteredtasks} {toggleDone} {removeTask}/>
</main>

<style>
    main {
        margin: 1rem auto;
        max-width: 800px;
    }
    .button-container{
        display: flex;
        justify-content: end;
        margin-bottom: 1rem;
        gap: 0.5rem;
    }
    .filterButton {
        text-transform: capitalize;
    }
</style>