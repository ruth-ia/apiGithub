<template>
    <div>
        <div>
        Introduzca el nombre de usuario: <input v-model="usuario" type="text" @keydown.enter="obtenerUsuario()">
        </div>
        <div v-if="error">
            El usuario no existe
        </div>
        <div v-if="datosValidos">
            <b-card>
                <b-img :src="datosValidos.avatar_url" fluid alt="Avatar"></b-img>
                <b-card-text>
                <h3>{{ datosValidos.login }}</h3> 
                <p><a :href="datosValidos.html_url">URL de Github</a></p>
                </b-card-text>
                <b-button @click="obtenerRepositorios()" variant="success">Mostrar repositorios</b-button>
            </b-card>
        </div>
        <div v-if="listadoRepositorios">
            <h2>Listado de repositorios</h2>
            <GitHubRepos :repositorio="repo" v-for="repo of listadoRepositorios" :key="repo.id"></GitHubRepos>
        </div>
    </div>
</template>

<script>

// Importar componente GitHubRepos
import GitHubRepos from './GitHubRepos.vue'

export default {
    name: 'GitHub',
    components: {
        //Importar componente GitHubRepos
        GitHubRepos
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            error: false,
            datosValidos: null,
            usuario: "",
            infoUsuario: "",
            listadoRepositorios: []
        }
    },
    methods: {
        obtenerUsuario: function() {
            // Función para obtener los datos de usuario de la API de GitHub
            let url = `https://api.github.com/users/${this.usuario}`
            fetch(url).then(respuesta => {
                if(respuesta.ok) {
                    return respuesta.json();
                } else {
                    throw new Error("Usuario no existe");
                }
            }).then (datos => {
                this.datosValidos = datos;
                this.listadoRepositorios = null;
                this.error = false;
            }).catch (() => {
                this.datosValidos = null;
                this.listadoRepositorios = null;
                this.error = true;
            })
            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            // var userAuth = process.env.VUE_APP_USERNAME || "user";
            // var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

        },
        obtenerRepositorios: function() {
            // Función para obtener los repositorios del usuario desde la API de GítHub
            // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario
            let url = `https://api.github.com/users/${this.usuario}/repos`
            fetch(url).then(respuesta => {
                if(respuesta.ok) {
                    return respuesta.json();
                } else {
                    throw new Error("Error al recuperar los repositorios del usuario");
                }
            }).then (datos => {
                this.listadoRepositorios = datos
                console.log("listadoRepositorios",this.listadoRepositorios)
                console.log("this", this)
                this.error = false;
            }).catch (() => {
                this.listadoRepositorios = null;
                this.error = true;
            })
            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            // var userAuth = process.env.VUE_APP_USERNAME || "user";
            // var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

        }
    }
}
</script>

<style scoped>
div {
    text-align: center;
}
</style>
