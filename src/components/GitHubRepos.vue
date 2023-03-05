<template>
    <div>
        <b-card>
            <b-card-text>
                <h3>{{repositorio.full_name}}</h3> 
                <p><a :href="repositorio.html_url">Enlace al repositorio</a></p>
                <p>{{ repositorio.description }}</p>
                <p>Número de forks: {{repositorio.full_name}}</p>
            </b-card-text>
        </b-card>
    </div>
</template>

<script>
export default {
    name: 'GitHubRepos',
    props: [
        "repositorio"
    ],
}
</script>

<style scoped>
div {
    text-align: center;
}
</style>
