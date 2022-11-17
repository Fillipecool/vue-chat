<template>
    <div contenteditable="true" class="w-96 m-12 h-80  border-neutral-800">
      <textarea @keydown.enter="getSentimental(), getWord()" type="text" v-model="msg" placeholder="Coloque um texto" class="w-96 h-60 border-2"/>
    </div>
</template>

<script>

import axios from "axios"

export default {
  name: 'myChat',
    data() {
      return {
        msg: "",
      }
    },
  methods: {
    getSentimental(){
      try{
        axios.get("https://test-nlp-api.alertrack.com.br/v1/polarity/unique/?sentence="+this.msg).then((response) => {
        console.log(response.data.describe)
      })
      }catch(error){
        console.log(error)
      }
    },
    getWord(){
      const palavras = this.msg.split(' ') 
      try{
        for (let index = 0; index < palavras.length; index++){
          axios.get('https://test-languagetools.alertrack.com.br/v2/check?language=pt-BR&text='+palavras[index])
          .then((response) =>{
              response.data.matches.length != 0 ? this.getCorrect(response.data.matches[0]) : ''
          })
        }
      }catch(error){
        console.log(error)
      }
    },
    getCorrect(param){
      console.log(param)
          // vue.http.interceptor.push({
          //   request: function (request)
          // })
      // vou precisar do shortMessage,sentence, message e [replacements]
      
    }
  },
}
</script>