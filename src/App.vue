<template>
    <div>
        <h1>Gestor de repositorios de Github</h1>
        <GitHub></GitHub>
    </div>
</template>

<script>
// TODO: Importar componente GitHub
import GitHub from './components/GitHub.vue'

export default {
    name: 'App',
    components: {
        GitHub
    }
}
</script>

<style>
div {
    text-align: center;
}
</style>
