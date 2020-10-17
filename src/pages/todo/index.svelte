<script>
    let currentValue = '';
    let tasks = [];
    $: numberUnchecked = tasks.filter(task => task.checked === false).length;
    $: numberChecked = tasks.length - numberUnchecked

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
    function addTask(e) {
        if (currentValue !== '') {
            let n = tasks.length;
            let lastId = n === 0 ? -1 : tasks[0].id;
            tasks.unshift({id: lastId + 1, value: currentValue, checked: false});
            tasks = tasks;
            currentValue = '';
        }
    }
    function checkTask(i) {
        let idInArray = findIdByAttr(tasks, 'id', i);
        tasks[idInArray].checked = !tasks[idInArray].checked;
        tasks = tasks;
    }
    function removeAllChecked() {
        tasks = tasks.filter(task => task.checked === false);
    }
</script>

<h1>To-do</h1>
<form>
    <input required type="text" name="newtask" id="newtask" placeholder="New Task" bind:value={currentValue}>
    <br>
    <button
        type="submit"
        on:click|preventDefault={addTask}
        disabled={currentValue === ""}>Add</button>
    <button
        on:click|preventDefault={removeAllChecked}
        disabled={numberChecked === 0}>Remove selected tasks</button>
</form>

{#if tasks.length === 0}
    <p>No task yet…</p>
{:else}
    {#if numberUnchecked === 0}
        <h2>All done!</h2>
    {:else}
        <h2>{numberUnchecked} {numberUnchecked === 1 ? "Task" : "Tasks"}</h2>
    {/if}
{/if}
<ul>
{#each tasks as {id, value, checked}}
    <li on:click={checkTask(id)}>
        <span class="check">{checked ? "☑" : ""}</span> 
        <span style={checked ? "text-decoration: line-through" : ""}>{value}</span>
    </li> 
{/each}
</ul>

<style>
    input {
        width: 300px;
    }

    ul {
        list-style-type: none;
    }

    .check {
        cursor: pointer;
        font-family: "todo-font";
        margin: 3px;
        position: relative;
        right: 5px;
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