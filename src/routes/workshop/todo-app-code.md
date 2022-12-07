---
title: 6. Todo App Code Checkpoints
section: workshop
order: 6
active: true
---

# Todo App Code Checkpoints

Below are checkpoints for the application build in the Frontend Masters's [Reactivity with SolidJS course](https://frontendmasters.com/courses/reactivity-solidjs/). The checkpoints line up with the lesson names in the course. For example, if you are stuck in the `Modifying Todos` lesson, you can find the completed code in the cooresponding checkpoint below.

### Lesson Checkpoints
1. [Building a Todo App](#checkpoint1)
2. [Creating New Todos](#checkpoint2)
3. [Modifying Todos](#checkpoint3)
4. [View Filters & Clearning Completed](#checkpoint4)

<h2 id="checkpoint1">Checkpoint 1: Building a Todo App</h2>

```javascript
// File: App.jsx
import { createSignal, Show } from 'solid-js';

const App = () => {
    const [todos, setTodos] = createSignal([
        {
            id: 1,
            title: "Learn SolidJS",
            completed: false
        }
    ]);
    return (
    <section class="todoapp">
      <header class="header">
        <h1>Todos</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
        />
      </header>
      <Show when={todos().length > 0}>
          <ul class="todo-list">
            <For each={filterTodos(todos())}>
              {(todo) => (
                <li class="todo">
                  <div class="view">
                    <input
                      type="checkbox"
                      class="toggle" />
                    <label>{todo.title}</label>
                    <button class="destroy" />
                  </div>
                </li>
              )}
            </For>
          </ul>
      </Show>
    </section>
  );
};

export default App;
```

<h2 id="checkpoint2">Checkpoint 2: Creating New Todos</h2>

```javascript
// File: App.jsx
import { createSignal, createMemo, Show, onCleanup } from 'solid-js';

let counter = 0;
const ESCAPE_KEY = 27;
const ENTER_KEY = 13;

const App = () => {
    const [todos, setTodos] = createSignal([]);

    const addTodo = (event) => {
        const title = event.target.value.trim();
        if (event.keyCode === ENTER_KEY && title) {
        setTodos((todos) => [
            ...todos,
            { id: counter++, title, completed: false },
        ]);
        event.target.value = '';
        }
    };

    return (
    <section class="todoapp">
      <header class="header">
        <h1>Todos</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
          onKeyDown={addTodo}
        />
      </header>
      <Show when={todos().length > 0}>
          <ul class="todo-list">
            <For each={filterTodos(todos())}>
              {(todo) => (
                <li class="todo">
                  <div class="view">
                    <input
                      type="checkbox"
                      class="toggle" />
                    <label>{todo.title}</label>
                    <button class="destroy" />
                  </div>
                </li>
              )}
            </For>
          </ul>
      </Show>
    </section>
  );
};

export default App;
```

<h2 id="checkpoint3">Checkpoint 3: Modifying Todos</h2>

```javascript
// File: App.jsx
import { createSignal, createMemo, Show } from 'solid-js';

let counter = 0;
const ESCAPE_KEY = 27;
const ENTER_KEY = 13;

const App = () => {
    const [todos, setTodos] = createSignal([]);
    const remainingCount = createMemo(
        () => todos().length - todos().filter((todo) => todo.completed).length
    );

    const addTodo = (event) => {
        const title = event.target.value.trim();
        if (event.keyCode === ENTER_KEY && title) {
        setTodos((todos) => [
            ...todos,
            { id: counter++, title, completed: false },
        ]);
        event.target.value = '';
        }
    };

    const toggle = (todoId) => {
        setTodos((todos) =>
        todos.map((todo) => {
            if (todo.id !== todoId) return todo;
            return { ...todo, completed: !todo.completed };
        })
        );
    };

    const remove = (todoId) => {
        setTodos((todos) => todos.filter((todo) => todoId !== todo.id));
    };

    const toggleAll = (event) => {
        const completed = event.target.checked;
        setTodos((todos) => todos.map((todo) => ({ ...todo, completed })));
    };

    return (
        <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input
            class="new-todo"
            placeholder="What needs to be done?"
            onKeyDown={addTodo}
            />
        </header>
        <Show when={todos().length > 0}>
            <section class="main">
                <input
                    id="toggle-all"
                    class="toggle-all"
                    type="checkbox"
                    checked={!remainingCount()}
                    onInput={toggleAll}
                />
                <label for="toggle-all" />
                <ul class="todo-list">
                    <For each={filterTodos(todos())}>
                    {(todo) => (
                        <li
                        class="todo"
                        classList={{
                            completed: todo.completed,
                        }}
                        >
                        <div class="view">
                            <input
                            type="checkbox"
                            class="toggle"
                            checked={todo.completed}
                            onInput={() => toggle(todo.id)}
                            />
                            <label>{todo.title}</label>
                            <button class="destroy" onClick={() => remove(todo.id)} />
                        </div>
                        </li>
                    )}
                    </For>
                </ul>
            </section>
        </Show>
        </section>
    );
};

export default App;
```


<h2 id="checkpoint4">Checkpoint 4: View Filters & Clearning Completed</h2>

This is the final step for building the Todo application. You can also view the completed code and test out the application [on Stackblitz](https://stackblitz.com/edit/github-uv4fun?file=src%2FApp.jsx)


```javascript
// File: App.jsx
import { createSignal, createMemo, Show, onCleanup } from 'solid-js';

let counter = 0;
const ESCAPE_KEY = 27;
const ENTER_KEY = 13;

const App = () => {
  const [todos, setTodos] = createSignal([]);
  const [showMode, setShowMode] = createSignal('all');
  const remainingCount = createMemo(
    () => todos().length - todos().filter((todo) => todo.completed).length
  );

  const addTodo = (event) => {
    const title = event.target.value.trim();
    if (event.keyCode === ENTER_KEY && title) {
      setTodos((todos) => [
        ...todos,
        { id: counter++, title, completed: false },
      ]);
      event.target.value = '';
    }
  };

  const toggle = (todoId) => {
    setTodos((todos) =>
      todos.map((todo) => {
        if (todo.id !== todoId) return todo;
        return { ...todo, completed: !todo.completed };
      })
    );
  };

  const remove = (todoId) => {
    setTodos((todos) => todos.filter((todo) => todoId !== todo.id));
  };

  const toggleAll = (event) => {
    const completed = event.target.checked;
    setTodos((todos) => todos.map((todo) => ({ ...todo, completed })));
  };

  const clearCompleted = () => {
    setTodos((todos) => todos.filter((todo) => !todo.completed));
  };

  const filterTodos = (todos) => {
    if (showMode() === 'active') return todos.filter((todo) => !todo.completed);
    else if (showMode() === 'completed')
      return todos.filter((todo) => todo.completed);

    return todos;
  };

  const locationHandler = () => {
    setShowMode(location.hash.slice(2) || 'all');
  };
  window.addEventListener('hashchange', locationHandler);
  onCleanup(() => window.removeEventListener('hashchange', locationHandler));

  return (
    <section class="todoapp">
      <header class="header">
        <h1>Todos</h1>
        <input
          class="new-todo"
          placeholder="What needs to be done?"
          onKeyDown={addTodo}
        />
      </header>
      <Show when={todos().length > 0}>
        <section class="main">
          <input
            id="toggle-all"
            class="toggle-all"
            type="checkbox"
            checked={!remainingCount()}
            onInput={toggleAll}
          />
          <label for="toggle-all" />
          <ul class="todo-list">
            <For each={filterTodos(todos())}>
              {(todo) => (
                <li
                  class="todo"
                  classList={{
                    completed: todo.completed,
                  }}
                >
                  <div class="view">
                    <input
                      type="checkbox"
                      class="toggle"
                      checked={todo.completed}
                      onInput={() => toggle(todo.id)}
                    />
                    <label>{todo.title}</label>
                    <button class="destroy" onClick={() => remove(todo.id)} />
                  </div>
                </li>
              )}
            </For>
          </ul>
        </section>
        <footer class="footer">
          <span class="todo-count">
            <strong>{remainingCount()}</strong>
            {remainingCount() === 1 ? ' item ' : '  items '}left
          </span>
          <ul class="filters">
            <li>
              <a href="#/" classList={{ selected: showMode() === 'all' }}>
                All
              </a>
            </li>
            <li>
              <a
                href="#/active"
                classList={{ selected: showMode() === 'active' }}
              >
                Active
              </a>
            </li>
            <li>
              <a
                href="#/completed"
                classList={{ selected: showMode() === 'completed' }}
              >
                Completed
              </a>
            </li>
          </ul>
          <Show when={remainingCount() !== todos().length}>
            <button class="clear-completed" onClick={clearCompleted}>
              Clear completed
            </button>
          </Show>
        </footer>
      </Show>
    </section>
  );
};

export default App;
```
