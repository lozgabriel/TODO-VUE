<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TasksProgress :progress="progress"/>
		<NewTaskVue @taskAdded="addTask"/>
		<TaskGrid :tasks="tasks"
		@taskDeleted="deleteTask"
		@taskStateChanged="toggleTaskState"/>
	</div>
</template>

<script>
import TasksProgress from './components/TasksProgress.vue';
import TaskGrid from './components/TaskGrid.vue'
import NewTaskVue from './components/NewTask.vue';
export default {
	components: {
		TaskGrid, NewTaskVue, TasksProgress
	},
	data() {
		return {
			tasks: []
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length
			//função t => !t.pending).length vai pegar qm não estiver pendente
			const done = this.tasks.filter(t => !t.pending).length
			// Se valor total vier com valor 0, irá ocorrer o problema NA, para isso usar || retornará o valor 0 para função
			return Math.round (done / total * 100) || 0
		}
	},
	watch: {
		//Sempre que o array mudar, ele vai chamar essa função
		tasks: {
			deep: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}

		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0
			if(reallyNew) {
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		deleteTask(id) {
			// Método splice remove o index pelo id e o 1 é a quantidade a ser excluida
			this.tasks.splice(id, 1)
		},
		toggleTaskState(id) {
			this.tasks[id].pending = !this.tasks[id].pending
		}
	},
	created() {
		const json = localStorage.getItem('tasks')
		const array = JSON.parse(json)
		if(Array.isArray(array)) {
			this.tasks = array
		} else {
			this.tasks = []
		}
	}
}
</script>
	
<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, #000, #673ab7);
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		margin: 0 20%;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 15px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
