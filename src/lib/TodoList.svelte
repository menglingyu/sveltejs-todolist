<script lang="ts">
    enum FilterType {
        All = "ALL",
        Doing = "DOING",
        Done = "DONE",
    }

    let newItem = "";
    let todos = [];
    let filterType = FilterType.All;

    const addTodo = () => {
        todos = [...todos, { label: newItem, done: false }];
        newItem = "";
    };

    const removeTodo = (index) => {
        todos.splice(index, 1);
        todos = todos;
    };

    $: filterTodos = todos;
    $: {
        switch (filterType) {
            case FilterType.All:
                filterTodos = todos;
                break;
            case FilterType.Doing:
                filterTodos = todos.filter((todo) => !todo.done);
                break;
            case FilterType.Done:
                filterTodos = todos.filter((todo) => todo.done);
                break;
            default:
                break;
        }
    }
    $: {
        console.log("todoList", todos);
    }
</script>

<div id="todolist">
    <h1>
        Todo List
        <span>Get things done, one item at a time.</span>
    </h1>
    <ul>
        {#if filterTodos.length}
            {#each filterTodos as item, index}
                <li class={item.done ? "done" : ""}>
                    <span class="label">{item.label}</span>
                    <div class="actions">
                        <input bind:checked={item.done} type="checkbox" />
                        <span on:click={() => removeTodo(index)}>❌</span>
                    </div>
                </li>
            {/each}
        {:else}
            <p v-else class="emptylist">Your todo list is empty.</p>
        {/if}
    </ul>

    <form name="newform" on:submit|preventDefault={addTodo}>
        <input type="text" name="newitem" id="newitem" bind:value={newItem} />
        <button type="submit">Add item</button>
    </form>
    <div class="control">
        {#each [{ type: FilterType.Done, label: "已完成" }, { type: FilterType.Doing, label: "未完成" }, { type: FilterType.All, label: "全部" }] as { type, label }}
            <span
                class={type === filterType ? "control-active" : ""}
                on:click={() => (filterType = type)}
            >
                {label}
            </span>
        {/each}
    </div>
</div>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    #todolist {
        margin: 4rem auto;
        padding: 2rem 3rem 3rem;
        max-width: 500px;
        background: #ff6666;
        color: #fff;
        box-shadow: -20px -20px 0px 0px rgba(100, 100, 100, 0.1);
    }
    #todolist h1 {
        /*text-align:center;*/
        font-weight: normal;
        font-size: 2.6rem;
        letter-spacing: 0.05em;
        border-bottom: 1px solid rgba(255, 255, 255, 0.3);
    }
    #todolist h1 span {
        display: block;
        font-size: 0.8rem;
        margin-bottom: 0.7rem;
        margin-left: 3px;
        margin-top: 0.2rem;
    }

    #todolist .emptylist {
        margin-top: 2.6rem;
        text-align: center;
        letter-spacing: 0.05em;
        font-style: italic;
        opacity: 0.8;
    }
    #todolist ul {
        margin-top: 2.6rem;
        list-style: none;
    }

    #todolist li {
        display: flex;
        margin: 0 -3rem 4px;
        padding: 1.1rem 3rem;
        justify-content: space-between;
        align-items: center;
        background: rgba(255, 255, 255, 0.1);
    }

    #todolist .actions {
        flex-shrink: 0;
        padding-left: 0.7em;
    }
    #todolist .label {
        position: relative;
        transition: opacity 0.2s linear;
    }
    #todolist .done .label {
        opacity: 0.6;
        text-decoration: line-through;
    }
    #todolist .done .label:before {
        content: "";
        position: absolute;
        top: 50%;
        left: -0.5rem;
        display: block;
        width: 0%;
        height: 1px;
        background: #fff;
        animation: strikeitem 0.3s ease-out 0s forwards;
    }

    /* FORM */
    form {
        margin-top: 3rem;
        display: flex;
        flex-wrap: wrap;
    }
    form label {
        min-width: 100%;
        margin-bottom: 0.5rem;
        font-size: 1.3rem;
    }
    form input {
        flex-grow: 1;
        border: none;
        background: #f7f1f1;
        padding: 0 1.5em;
        font-size: initial;
    }
    form button {
        padding: 0 1.3rem;
        border: none;
        background: #ff6666;
        color: white;
        text-transform: uppercase;
        font-weight: bold;
        border: 1px solid rgba(255, 255, 255, 0.3);
        margin-left: 5px;
        cursor: pointer;
        transition: background 0.2s ease-out;
    }
    form button:hover {
        background: #ff5e5e;
    }
    form input,
    form button {
        font-family: "Quicksand", sans-serif;
        height: 3rem;
    }
    .control {
        display: flex;
        margin-top: 20px;
    }
    .control span {
        margin: 0 8px;
        cursor: pointer;
    }
    .control-active {
        color: midnightblue;
    }
</style>
