<script lang="ts">
    let newTask = $state(''); // rune to keep track of
    let {addTask} : { addTask: (newTask: string) => void} = $props();
    function formSubmitted(e: SubmitEvent){
        e.preventDefault();
        console.log("--newtask", newTask);
        addTask(newTask); //pass the task to parent props so that parent will handle rest
        newTask = ""; //clear
    }
</script>
<form onsubmit={formSubmitted}>
    <label>
        Task
        <input name="newtask" bind:value={newTask}/>
    </label>
    <div class="button-container">
        <button>Add</button>
    </div>
</form>
<style>
    form{
        margin-bottom: 1rem;
    }
    .button-container {
        display: flex;
        justify-content: end;
    }
</style>