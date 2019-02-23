<template lang="html">
    <i class="material-icons">{{ typeIcon }}</i>
</template>

<script>
// Componente 'app-icon'
export default {
    props: [ 'type' ],
    computed: {
        typeIcon() {
            return this .type;
        }
    }
}
</script>

<style lang="css" scoped>
</style>
