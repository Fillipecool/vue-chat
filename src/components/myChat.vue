<template>
  <div>
    <p class="p-8 m-5 "> {{ msg }}</p>
  </div>
    <input type="text" v-model="input_msg" placeholder="Coloque um texto" class="p-8 m-5 border-2 border-neutral-800">
    <input type="submit" name="Enviar" v-on:click="send" class="p-8 m-5 border-4 hover:border-black">
    <picComponent class="w-10"/>
  <div>
    <button class="border-2 bg-blue-100 m-2 p-3 w-40 hover:bg-blue-300 rounded-xl" @click="Informacao">{{ saber }}</button>
    <p class="rounded-2xl m-3 w-60 bg-green-300 p-2" v-show="Info">Lorem ipsum dolor sit amet consectetur adipisicing elit. Maxime quasi odit, reiciendis debitis, rem corrupti consectetur aperiam unde incidunt reprehenderit suscipit nam ducimus deleniti quam. Quos vel modi praesentium ea! </p>
  </div>
</template>

<script>
  import picComponent from './picComponent.vue'
  export default {
  name: 'myChat',
  components: {
    picComponent
    },
    data() {
      return {
        msg: "",
        input_msg: "",
        Info: false,
        saber: "Mostrar Informação"
      }
    },
  methods: {
    Informacao() {
      if(!this.Info){
        this.saber = "Ocultar Informação"
      }else{
        this.saber = "Mostrar Informação"
      }
      this.Info = !this.Info
    },
    send() {
      console.log("chego aqui")  
      this.msg = this.input_msg
    }
  }
}
</script>