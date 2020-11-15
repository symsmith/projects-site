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
    <ul>
    {#each $tasks as {id, value, checked}}
        <li on:click={checkTask(id)}>
            <span class="check icon">{checked ? "☑" : ""}</span>
            <span style={checked ? "text-decoration: line-through" : ""}>{value}</span>
        </li>
    {/each}
    </ul>
</div>

<style>
    ul {
        list-style-type: none;
    }

    .check {
        cursor: pointer;
        font-family: "todo-font";
        margin: 3px;
        position: relative;
        right: 5px;
        top: 1px;
    }

    @font-face {
        font-family: "todo-font";
        src: url("/font/todo-font.ttf") format("ttf"),
             url("/font/todo-font.woff") format("woff"),
             url("/font/todo-font.eot") format("eot"),
             url("/font/todo-font.svg") format("svg"),
             url("/font/todo-font.woff2") format("woff2");
    }
</style>