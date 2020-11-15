<script>
    import {tasks} from './store.js';

    let currentValue = '';
    $: numberUnchecked = $tasks.filter(task => task.checked === false).length;
    $: numberChecked = $tasks.length - numberUnchecked;

    function addTask(e) {
        if (currentValue !== '') {
            let n = $tasks.length;
            let lastId = n === 0 ? -1 : $tasks[0].id;
            $tasks.unshift({id: lastId + 1, value: currentValue, checked: false});
            $tasks = $tasks;
            currentValue = '';
        }
    }

    function removeAllChecked() {
        $tasks = $tasks.filter(task => task.checked === false);
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

<style>
    input {
        width: 300px;
    }
</style>
