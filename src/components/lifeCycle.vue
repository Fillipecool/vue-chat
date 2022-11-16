<template>
    <h1>Testanto {{ info }}</h1>
</template>

<script>
export default{
    name: 'lifeCycle',
    data(){
        return{
            info:"placeholder"
        }
    },
    
    created(){
        console.log(this.info);
        setTimeout(() => {
            this.info = "created"
        }, 2000)
    },
    mounted() {
        setTimeout(() => {
            this.info = "acabo"
        }, 4000)
    },
}
</script>