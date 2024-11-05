<template>
    <div>
        <h1>Lista de Tareas</h1>

        <!-- Botón para cargar las tareas existentes -->
        <button @click="fetchTasks">Cargar Tareas</button>

        <!-- Formulario para agregar una nueva tarea -->
        <form @submit.prevent="addTask">
            <input
                type="text"
                v-model="newTask"
                placeholder="Nueva tarea"
                required
            />
            <button type="submit">Agregar Tarea</button>
        </form>

        <!-- Lista de tareas -->
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
        };
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
            const task = {
                id: this.tasks.length + 1,  
                todo: this.newTask,         
                completed: false,           
            };

            // Agregar la tarea al array de tareas
            this.tasks.push(task);

            // Limpiar el campo de entrada
            this.newTask = "";
        },

        // Cambiar el estado de una tarea (completada/no completada)
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },

        // Eliminar la tarea seleccionada
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
    },
};
</script>

<style scoped>
input[type="text"] {
    margin-right: 10px;
}
button {
    margin-right: 5px;
}
</style>
