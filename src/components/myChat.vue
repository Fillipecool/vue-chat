<template>
    <div  class="w-4/5 h-48  absolute bottom-10 left-10">
      <textarea v-model="msg" rows="10" cols="125" @keydown.enter="getSentimental(), getWord()" class="resize-none absolute placeholder:italic placeholder:text-slate-400 block bg-white border border-slate-300 rounded-md z-40 shadow-sm focus:outline-none focus:border-sky-500 focus:ring-sky-500 focus:ring-1 sm:text-sm text-start" placeholder="Escreva alguma coisa" type="text" name="textchat"/>
    </div>
    <div class="bg-red-500 bg border-neutral-800"> 
      <button id="renderResults" type="button" @click="getCorrect(), getOffset()" v-if="info != 0"
      > 
        {{ info }}
      </button> 
    </div> 
    <div v-for="(suggestions, index) in suggestions" :key="index">
      <button id="replace" type="button" v-if="suggestions != 0" class="border-2 no-underline hover:underline"
      > 
        {{ suggestions }}
      </button>
    </div>
   <display-correct :msg="msg" >

   </display-correct>
</template>


<script>
import axios from "axios"
import DisplayCorrect from './DisplayCorrect.vue'
export default {
  name: 'myChat',
  components: {
    DisplayCorrect
  },
    data() {
      return {
        msg: "",
        info: [],
        suggestions:[],
        wrong:[]
        
        }
      },
  methods: {
    funcao(){
      let stringExample = this.msg;
      let result = stringExample.substring(0, 4);
      console.log("funcao",result)
    },

    getOffset(){
      axios.get('https://test-languagetools.alertrack.com.br/v2/check?language=pt-BR&text='+this.msg)
      .then((response) =>{
        this.offset = response.data.matches.forEach(offset => {
          console.log("offset", offset)
        })
      })
      .catch(error => console.log(error))
    },  

    getIssueType(){
      // < 1° erro :  typographical (verificar letra maiuscula) / id = CASING
      // < 2° erro :  misspelling   (Erro ortográfico) / id = HUNSPELL_RULE
      // < 3° erro : duplication    (Possivel escrita repitida)  / id = PORTUGUESE_WORD_REPEAT_RULE
    },
    getCorrect(){
          axios.get('https://test-languagetools.alertrack.com.br/v2/check?language=pt-BR&text='+this.msg)
          .then((response) => {
            this.info = response.data.matches
            this.info.forEach(wrong => {
              console.log("foreach palavra errada",wrong)
            })
          })
          .catch(error => console.log(error))
        },
    /**
     * getSentimental irá verificar o sentimento
     * da FRASE para entender se é negativa, Neutra ou Positiva
     */
    getSentimental(){
      axios.get("https://test-nlp-api.alertrack.com.br/v1/polarity/unique/?sentence="+this.msg)
      .then((response) => {
        console.log("Sentimento : ",response.data.describe)
      })
      .catch(error => console.log(error))
    },

    /**
     * getWord verificará cada palavra errada
     * separada por espaço
     * e devolverá a requisição das erradas para serem corrigidas
     */
    getWord(){
      axios.get('https://test-languagetools.alertrack.com.br/v2/check?language=pt-BR&text='+this.msg)
      .then((response) => {
          // console.log('aqui', response.data.matches[0].sentence)
          if(response.data.matches.length !== 0 && response.data.matches[0].sentence !== null){
            this.info = response.data.matches[0].sentence
            console.log("getWord",response.data.matches[0].offset)
        }
      })
      .catch(error => console.log(error))
    }
  },
}

</script>