<template>
    <div class="container">

        <div class="mx-auto" style="width: 70%;">
            <h2 class="center">Tareas con Vue 2</h2>
        </div>

        <div id="app" class="mx-auto card m-4" style="width: 70%;">
            <div class="card-body">
                <h5 class="card-title">Listado de tareas</h5>

                <ul class="list-group tasks">
                    <li is="app-task" v-for="(task, index) in tasks" :task="task" :index="index" :key="task .id" @remove="deleteTask"></li>
                </ul>
                <a @click="deleteTasksCompleted()" href="#">Eliminar todas las tareas completadas</a>
                <br>
                <form @submit.prevent="createTask" class="form-row">
                    <div class="col-9">
                        <input v-model="new_task" class="form-control" type="text">
                    </div>
                    <div class="col-3 ">
                        <button class="btn btn-primary w-100">Agregar tarea</button>
                    </div>
                </form>
            </div>
        </div>

        <footer class="mx-auto" style="width: 70%;">
            <p>2019 - Aprendiendo Vue 2 con Styde</p>
        </footer>

    </div>
</template>

<script>
// Instancia de aplicación "Listado de Tareas"
export default {
    // Define la 'data' como una función por que se trata de un componente
    data: function() {
        return {
            new_task: '',
            tasks: [
                {
                    description: 'Aprender Foundation',
                    pending: true
                },
                {
                    description: 'Aprender Angular',
                    pending: true
                },
                {
                    description: 'Aprender Gulp',
                    pending: false
                },
                {
                    description: 'Aprender Vue',
                    pending: false
                }
            ]
        }
    },
    methods: {
        createTask() {
            this .tasks .push({
                description: this .new_task,
                pending: true,
                editing: false
            });
            this .new_task = '';
        },
        deleteTask( index ) {                             // 'index' hace parte de la deficinion del componente 'app-task'
            this .tasks .splice( index, 1 );              // Elimina la tarea del Array con el indice indicado 'index'
        },
        deleteTasksCompleted() {
            this .tasks = this .tasks .filter( ( task ) => {
                return task .pending;
            });
        }
    }
}
</script>

<style lang="scss"></style>
