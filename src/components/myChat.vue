<template>
    <textarea @keydown.enter="getSentimental(), getWord()" type="text"  v-model="msg" placeholder="Coloque um texto" class=" z-40 w-96 h-40 border-2"/> 

    <div v-for="info in info" :key="info" class="w-60 bg-red-500 bg border-neutral-800"> 
      <button id="corretor" type="button" @click="getCorrect(suggestions)" v-if="info != null">{{ info }}</button>
      <span class="border-2"> {{ suggestions }} </span>
    </div> 
    
    <!-- <button v-show="" type="button"> {{ suggestions }} </button> -->
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
        suggestions:[]
        }
      },
  methods: {
    /**
     * getSentimental irá verificar o sentimento
     * da FRASE para entender se é negativa, Neutra ou Positiva
     */
    getSentimental(){
        axios.get("https://test-nlp-api.alertrack.com.br/v1/polarity/unique/?sentence="+this.msg)
        .then((response) => {
        console.log(response.data.describe)
      })
      .catch(error => console.log(error))
    },

    /**
     * getWord verificará cada palavra errada
     * separada por espaço
     * e devolverá a requisição das erradas para serem corrigidas
     */
    getWord(){
      const palavras = this.msg.split(' ')
        for (let index = 0; index < palavras.length; index++){
          axios.get('https://test-languagetools.alertrack.com.br/v2/check?language=pt-BR&text='+palavras[index])
          .then((response) => {
            // console.log('aqui', response.data.matches[0].sentence)
            if(response.data.matches.length !== 0 && response.data.matches[0].sentence !== null){
              this.info[index] = response.data.matches[0].sentence
            }
          })
          .catch(error => console.log(error))
        }
    },
    getCorrect(){
      const correto = this.msg.split(' ')
        for (let index = 0; index < correto.length; index++){
          axios.get('https://test-languagetools.alertrack.com.br/v2/check?language=pt-BR&text='+correto[index])
          .then((response) => {
            // console.log('aqui', response.data.matches[0])
              this.suggestions = response.data.matches[0].replacements
              console.log(this.suggestions)
          })
          .catch(error => console.log(error))
        }
    },
  },
}
</script>