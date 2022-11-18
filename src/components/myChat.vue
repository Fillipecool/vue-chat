<template>
    <textarea @keydown.enter="getSentimental(), getWord()" type="text"  v-model="msg" placeholder="Coloque um texto" class="z-40 absolute w-96 h-60 border-2"/> 
    <div contenteditable="true" class="absolute w-96 h-60 bg-red-500 text-transparent bg border-neutral-800"> 
    {{ msg }}
    </div> 
</template>

<script>

import axios from "axios"

export default {
  name: 'myChat',
    data() {
      return {
        msg: "",
        erros: "",
        suggestions: {
          type: [Array, Object],
          default() {
            return []
          }
        }
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
              response.data.matches.length != 0 ? this.getCorrect(response.data.matches[0].sentence) : ''
          })
        }
      }catch(error){
        console.log(error)
      }
    
    },
    getCorrect(param){

      console.log('chegou', param)

    },
  },
}
</script>