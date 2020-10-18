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

<div class="container">
    <h1 class="title">To-do</h1>
    <form>
        <div class="field">
            <div class="control">
                <input
                    class="input is-fullwidth"
                    required
                    autofocus
                    type="text"
                    name="newtask"
                    id="newtask"
                    placeholder="New Task"
                    bind:value={currentValue}>
            </div>
        </div>
        <div class="field level is-mobile">
            <div class="control level-left">
                <button
                    class="button is-primary"
                    on:click|preventDefault={addTask}
                    disabled={currentValue === ""}>
                    Add
                </button>
            </div>
            <div class="control level-right">
                <button
                    class="button"
                    on:click|preventDefault={removeAllChecked}
                    disabled={numberChecked === 0}>
                    Remove selected tasks
                </button>
            </div>
        </div>
    </form>
</div>

<div class="section">
    {#if tasks.length === 0}
        <h3 class="subtitle">No task yet…</h3>
    {:else}
        {#if numberUnchecked === 0}
            <h3 class="subtitle">All done!</h3>
        {:else}
            <h3 class="subtitle">{numberUnchecked} {numberUnchecked === 1 ? "Task" : "Tasks"}</h3>
        {/if}
    {/if}
    <ul>
    {#each tasks as {id, value, checked}}
        <li on:click={checkTask(id)}>
            <span class="check icon">{checked ? "☑" : ""}</span> 
            <span style={checked ? "text-decoration: line-through" : ""}>{value}</span>
        </li> 
    {/each}
    </ul>
</div>

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