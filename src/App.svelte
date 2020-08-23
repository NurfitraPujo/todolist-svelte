<script>
  import { each, text, bubble, svg_element } from "svelte/internal";
  
    let todoItems = [];
    let newTodo = '';
  
    const addTodo = () => {
      newTodo = newTodo.trim();
      if (!newTodo) return;
  
      const todo = {
        text: newTodo,
        checked: false,
        id: Date.now()
      }
  
      todoItems = [...todoItems, todo];
      newTodo = ''
    }

    const toogleDone = (id) => {
      const index = todoItems.findIndex(item => item.id === Number(id));
      todoItems[index].checked = !todoItems[index].checked;
    }

    const deleteTodo = (id) => {
      todoItems = todoItems.filter(item => item.id !== Number(id));
    }

  </script>
  
  <main>
    <div class="container">
      <h1 class="app-title">todos</h1>
      <ul class="todo-list">
        {#each todoItems as todo (todo.id)}
        <li class="todo-item {todo.checked? 'done' : ''}">
          <input type="checkbox" id={todo.id}>
          <label for={todo.id} class="tick" on:click={() => toogleDone(todo.id)}></label>
          <span>{todo.text}</span>
          <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
            <svg><use href="#delete-icon"></use></svg>
          </button>
        </li>
        {/each}
      </ul>
      <div class="empty-state">
        <svg class="checklist-icon"><use href="#checklist-icon"></use></svg>
        <h2 class="empty-state___title">Add your first todo</h2>
        <p class="empty-state__description">What do you want to get done?</p>
      </div>
      <form on:submit|preventDefault={addTodo}>
        <input 
          type="text" 
          name="input" 
          id="todo-input" 
          class="js-todo-input" 
          aria-label="Enter a new todo item" 
          placeholder="E.g Build Web App"
          bind:value={newTodo}
          />
  
      </form>
    </div>
  </main>
  
  <style>
    main {
      text-align: center;
      padding: 1em;
      max-width: 240px;
      margin: 0 auto;
    }
  
    h1 {
      color: #ff3e00;
      text-transform: uppercase;
      font-size: 4em;
      font-weight: 100;
    }
  
    @media (min-width: 640px) {
      main {
        max-width: none;
      }
    }
  </style>