<script>
    import {tasks} from './store.js';

    $: numberUnchecked = $tasks.filter(task => task.checked === false).length;

    function toggleAll() {
        for (let task of $tasks) {
            task.checked = (numberUnchecked === 0) ? false : true;
        }
        $tasks = $tasks;
    }
</script>

<div class="section">
    {#if $tasks.length === 0}
        <h3 class="subtitle">No task yet…</h3>
    {:else}
        {#if numberUnchecked === 0}
            <h3 class="subtitle">All done!</h3>
        {:else}
            <h3 class="subtitle">{numberUnchecked} {numberUnchecked === 1 ? "Task" : "Tasks"}</h3>
        {/if}
    {/if}
    {#if $tasks.length > 0}
        <label class="checkbox" for="task-all">
            <input on:click={e => toggleAll()} checked={numberUnchecked === 0} id="task-all" type="checkbox">
            <span><strong>Check all</strong></span>
        </label>
    {/if}
    {#each $tasks as {id, value, checked}}
        <label class="checkbox" for="task-{id}">
            <input bind:checked={checked} id="task-{id}" type="checkbox">
            <span style={checked ? "text-decoration: line-through" : ""}>{value}</span>
        </label>
    {/each}
</div>

<style>
    label {
        display: block;
        margin-bottom: 10px;
        word-break: break-all;
    }

    span {
        padding-left: 10px;
    }
</style>