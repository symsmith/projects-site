<script>
    import {tasks} from './store.js';

    $: numberUnchecked = $tasks.filter(task => task.checked === false).length;

    function findIdByAttr(arr, attr, value) {
        let i = arr.length;
        let found;
        while(i--){
            if( arr[i]
                && arr[i].hasOwnProperty(attr)
                && (arguments.length > 2 && arr[i][attr] === value ) ) {
                    found = i;
            }
        }
        return found;
    }
    function checkTask(i) {
        let idInArray = findIdByAttr($tasks, 'id', i);
        $tasks[idInArray].checked = !$tasks[idInArray].checked;
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