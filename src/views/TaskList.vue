<template>
    <div>
        <h1>Lista de Tareas</h1>
        <div v-if="showAddTaskForm" class="add-task-form">
            <input v-model="newTask" placeholder="Nueva tarea" />
            <button @click="addTask">Agregar Tarea</button>
        </div>

        <button @click="fetchTasks">Cargar Tareas</button>

        <div v-if="tasks.length > 0">
            <div v-for="task in tasks" :key="task.id">
                <div>
                    <h5 :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.todo }}</h5>
                    <span>{{ task.completed ? 'Completada' : 'Pendiente' }}</span>
                    <button @click="toggleTaskCompletion(task)">
                        {{ task.completed ? 'Desmarcar' : 'Completar' }}
                    </button>
                    <button @click="deleteTask(task)">Eliminar</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "TaskList",
    data() {
        return {
            tasks: [],
            newTask: "",         
            showAddTaskForm: false 
        };
    },
    mounted() {
        if (this.$route.query.addTask === 'true') {
            this.showAddTaskForm = true;
        }
    },
    methods: {
        async fetchTasks() {
            try {
                const response = await axios.get('https://dummyjson.com/todos');
                this.tasks = response.data.todos;
            } catch (error) {
                console.error("Error al cargar las tareas:", error);
            }
        },
        addTask() {
            if (this.newTask.trim()) {
                // Crear una nueva tarea con un ID único y agregarla a la lista
                const task = {
                    id: this.tasks.length + 1,
                    todo: this.newTask,
                    completed: false,
                };
                this.tasks.push(task);

                // Limpiar el campo de entrada y ocultar el formulario
                this.newTask = "";
                this.showAddTaskForm = false;
            }
        },
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
    },
};
</script>

<style scoped>
.add-task-form {
    margin-bottom: 20px;
}
</style>
