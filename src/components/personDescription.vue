<template>
    <div>
        <h2 v-if="Status">Status : {{ Description }}</h2>
        <p v-else>Status : off</p>
    </div>
    <p>Para acessar teste clicar <a class="text-blue-600 hover:text-blue-800" v-bind:href="Link">aqui</a></p>
</template>

<script>

    export default{
        name: 'personDescription',
        data(){
            return{
                Description: "On",
                Status: true,
                Link: "https://google.com"
            }
        }
    }

</script>