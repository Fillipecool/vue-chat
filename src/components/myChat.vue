<template>
    <textarea @keydown.enter="getSentimental(), getWord()" type="text"  v-model="msg" placeholder="Coloque um texto" class=" z-40 w-96 h-40 border-2"/> 
    <ul v-for="info in info" :key="info" class="w-60 bg-red-500 bg border-neutral-800"> 
    <li type="button" onclick="getCorrect()" v-if="info != null">{{ info }}</li>
    </ul> 
</template>


<script>
import axios from "axios"
export default {
  name: 'myChat',
    data() {
      return {
        msg: "",
        erros: "",
        info: [],
        suggestions: []
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
          .then((response) => {
            console.log('aqui', response.data.matches[0].sentence)
            if(response.data.matches.length !== 0 && response.data.matches[0].sentence !== null){
              this.info[index] = response.data.matches[0].sentence
            }
              console.log(this.info)
          })
        }
      }catch(error){
        console.log(error)
      }
    },
    getCorrect(){
      
    },
    // getReplacement(){
    //   this.getCorrect()
    // }
  },
}
</script>