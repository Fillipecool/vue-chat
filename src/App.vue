<template>
    <myChat />
    <inputComponent />
</template>

<script>
  import myChat from './components/myChat.vue'
  import inputComponent from './components/inputComponent.vue';
  export default {
    name: 'App',
    components: {
      myChat,
      inputComponent
    }
  }
</script>