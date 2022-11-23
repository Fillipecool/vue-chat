<template>
    <div class="w-4/5 h-48  absolute bottom-10 left-10">
      <input @keydown.enter="getSentimental(), getWord()" v-model="msg" class="absolute placeholder:italic placeholder:text-slate-400 block bg-white w-full border border-slate-300 rounded-md z-40 w-4/5 h-48 shadow-sm focus:outline-none focus:border-sky-500 focus:ring-sky-500 focus:ring-1 sm:text-sm text-start" placeholder="Escreva alguma coisa" type="text" name="textchat"/>
    </div>
    <div v-for="info in info" :key="info" class="w-60 bg-red-500 bg border-neutral-800"> 
      <button id="corretor" type="button" @click="getCorrect()" v-if="info != null"> 
        {{ info }}
      </button> 
    </div> 
    <div v-for="suggestion in suggestions" :key="suggestion">
      <button id="replace" type="button" v-if="suggestions != 0" class="border-2 no-underline hover:underline"> 
        {{ suggestions }}
      </button>
    </div>
</template>


<script>
import axios from "axios"

export default {
  name: 'myChat',
    setup(){

    },
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
              console.log("sugestão", this.suggestions)
          })
          .catch(error => console.log(error))
        }
    },
  },
}
</script>