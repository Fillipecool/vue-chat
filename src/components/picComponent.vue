<template>
    <img :src="avatar" :alt="descr">
</template>

<script>
export default{
    name: 'picComponent',
    data (){
        return{
            avatar: "/img/sacd.jpg",
            descr: "descrição da imagem"
        }
    }
}
</script>