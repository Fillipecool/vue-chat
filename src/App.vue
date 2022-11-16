<template>
    <lifeCycle />
    <myChat />
    <personDescription />
</template>

<script>
  import myChat from './components/myChat.vue'
  import lifeCycle from './components/lifeCycle.vue'
  import personDescription from './components/personDescription.vue'
  export default {
    name: 'App',
    components: {
      myChat,
      lifeCycle,
      personDescription
    }
  }
</script>