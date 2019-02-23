<template>
    <li class="list-group-item d-flex justify-content-between align-items-center">
      <a @click="toggleStatus" class="btn btn-link btn-sm" :class="!task .pending ? 'checkbox-disabled' : ''" href="#">
          <app-icon :type="task .pending ? 'check_box_outline_blank' : 'check_box'"></app-icon>
      </a>

      <template v-if="!editing">
          <span class="pl-2 pr-2 flex-grow-1" :class="{ editing: editing, completed: !task .pending }">{{ task .description }}</span>
          <span>
              <a @click="edit" class="btn btn-link btn-sm" :class="!task .pending ? 'disabled' : ''" href="#"><app-icon type="edit"></app-icon></a>
              <a @click="remove" class="btn btn-link btn-sm" :class="!task .pending ? 'disabled' : ''" href="#"><app-icon type="delete"></app-icon></a>
          </span>
      </template>
      <template v-else>
          <input class="w-75" type="text" v-model="draft">
          <span>
              <a @click="update" class="btn btn-link btn-sm" :class="!task .pending ? 'disabled' : ''" href="#"><app-icon type="done"></app-icon></a>
              <a @click="discard" class="btn btn-link btn-sm" :class="!task .pending ? 'disabled' : ''" href="#"><app-icon type="close"></app-icon></a>
          </span>
      </template>
    </li>
</template>

<script>
import EventBus from './event-bus.js';
import Icon from './Icon.vue';

// Componente 'app-task'
export default {
    // Registra componentes locales dentro de otro componente
    components: {
        'app-icon': Icon
    },
    data: function() {              // Definición de la data (propiedades) de un componente unicamente
        return {
            editing: false,
            draft: ''
        };
    },
    template: '#task-template',
    props: [ 'task', 'index' ],     // task e index están disponibles dentro del componente (por lo que podemos hacer referencia a task usando this)
    created() {
        // Escucha Evento
        EventBus .$on( 'editing', ( index ) => {
            if( index != this .index ) {
                this .discard();
                console .log( 'Discarting: ' + this .index );
            }
        });
    },
    methods: {
        edit() {
            // Emite un evento: Evita que pueda editar multiples tareas
            console .info( 'Editing: ' + this .index  );
            EventBus .$emit( 'editing', this .index );

            this .draft = this .task .description;   // Crea Borrador de la tarea
            this .editing = true;
        },
        toggleStatus() {
            this .task .pending = !this .task .pending;
        },
        update() {
            this .task .description = this .draft;   // Actualiza la descripción de la tarea
            this .editing = false;
        },
        discard() {
            this .editing = false;
        },
        remove() {
            this .$emit( 'remove', this .index );       // Emite un evento de nombre 'remove' y el argumento de dicho evento (índice de la tarea a eliminar)
            console .log( 'Crea evento personalizado "remove" y lo lanza o ejecuta' );
        }
    }
}
</script>

<style lang="css" scoped>
</style>
