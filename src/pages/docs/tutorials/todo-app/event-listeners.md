---
title: "Event Listeners"
description: "Foo bar."
buttonTitle: "I added the event listeners"
parentId: "todo-app"
layout: "tutorial"
time: 90
weight: 6
---

## {$page.title}

So now you should have a static list of todo items, what now? Remember the end
goal is to be able to click the todos to mark them as completed, so let's start
with adding a click event listener to the list items.

```text/jsx
// TodoItem.js

class TodoItem extends JSXComponent {
	render() {
		const elementClasses = `todo-item${this.props.todo.done ?
			' todo-item-done' : ''}`;

		return (
			<li
				class={elementClasses}
				data-onclick={this.handleClick.bind(this)}
			>
				{this.props.todo.title}
			</li>
		);
	}

	handleClick(event) {
		alert(this.props.todo.title);
	}
}
```

Now you should see an alert with the title of the clicked todo. Now you must
notify `TodoApp` that a todo was marked as completed so that it can update
the data.

This can be done by emitting a custom event with the info needed to make the
change. In this case we'll use the index value from PROPS.

```text/jsx
// TodoItem.js

class TodoItem extends JSXComponent {
	...

	handleClick(event) {
		this.emit('todoClick', {
			index: this.props.index
		});
	}
}
```

Now that the `TodoItem` is emitting an event, you must add a listener from the
parent component `TodoApp`.

```text/jsx
// TodoApp.js

class TodoApp extends JSXComponent {
	render() {
		return (
			<div class="todo-app">
				<ul>
					{this.state.todos.map((todo, index) => {
						return (
							<TodoItem
								events={{
									todoClick: this.handleTodoClick.bind(this)
								}}
								index={index}
								todo={todo}
							/>
						);
					})}
				</ul>
			</div>
		);
	}

	handleTodoClick(event) {
		alert(event.index);
	}
}
```

At this point you should have an event handler that fires every time a todo
item is clicked on. Next you will use this data to update the state
in `TodoApp`.
