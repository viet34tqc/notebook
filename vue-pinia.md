# Pinia

Pinia is a state management library of Vue

## Concept

### `defineStore`

First, we need to define a store using `defineStore` function. There are two params:

- Name of the store. This is unique
- An object that defines state of the store

The name convention of store is like React Hook
```	js
const useTaskStore = defineStore('taskStore', {
	state: () => {
		tasks: []
	}
})
```

### `getters`

getters function is like `computed`, it returns a value based on defined state.

```	js
const useTaskStore = defineStore('taskStore', {
	state: () => {
		tasks: []
	},
	getters: {
		completedTasks: (state) => {}
		// In order to access other getter, we need to use normal function instead of arrow function because `this` is undefined in arrow function
		otherCompletedTasks() {
			this.completedTasks
		} 
	}
})
```

### Actions

We use actions to mutate the state

```	js
const useTaskStore = defineStore('taskStore', {
	state: () => {
		tasks: []
	},
	actions: {
		addTask(task) {
			this.tasks.push(task);
		},
		// Actions can be asynchronous
		async getTask() {
			const res = await axios.get('getTask')
			if (res.data) {
				this.tasks = res.data
			}
		}
		async deleteTasks(task) {
			await axios.delete(task)
		}
	}
})
```

## Other way to define a store using Composition API style

```js
const useTaskStore = defineStore(() => {
	const tasks = ref([]);
	const completedTasks = () => computed(() => tasks.value.filtered(task => task.completed:))
	function addTask(task) {
		tasks.value.push(task);
	}
	async function getTask() {
		const res = await axios.get('getTask')
		if (res.data) {
			tasks.value = res.data
		}
	}
	async function deleteTask(task) {
		await axios.delete(task)
	}

	return {tasks, completedTasks, getTask, deleteTask}
})
```

## How to use pinia store in Vue Component

For convenience, if you want to destructure **reactive values** (not actions), you need to use `storeToRef(storeName)`. Actions are destructured without using `storeToRef`

```js
const taskStore = useTaskStore();
const {state, completedTasks, otherCompletedTasks} = storeToRef(taskStore)
const {addTask} = taskStore
```