<template>
  <h1>Jogo da Forca</h1>

  <section v-if="tela === 'inicio'" id="inicio"></section>

  <Formulario 
    v-if="etapa === 'palavra'" 
    title="Defina a palavra" 
    button="Próximo"
    :action="setPalavra" />

  <Formulario 
    v-if="etapa === 'dica'" 
    title="Defina a dica" 
    button="Iniciar o jogo" 
    :action="setDica"/>

  <section v-if="tela === 'jogo'" id="jogo">
    <Jogo 
      :erros="erros" 
      :palavra="palavra" 
      :etapa="etapa" 
      :dica="dica" 
      :verificarLetra="verificarLetra"
      :letras="letras"
      :jogar="jogar"
      :jogarNovamente="jogarNovamente"
    />
  </section>

</template>

<script>
import './style/global.css'
import Formulario from './components/Formulario.vue'
import Jogo from './components/Jogo.vue'

export default {
  name: 'App',
  data(){
    return{
      tela: 'inicio',
      etapa: 'palavra',
      palavra: '',
      dica: '',
      erros: 0,
      letras: []
    }
  },
  components: {
    Formulario,
    Jogo
  },
  methods: {
    setPalavra(palavra){
      this.palavra = palavra
      this.etapa = 'dica'
    },
    setDica(dica){
      this.dica = dica
      this.tela = 'jogo'
      this.etapa = 'jogo'
    },
    verificarLetra(letra){
      return this.letras.find(item => item.toLowerCase() === letra.toLowerCase())
    },
    // Adicionar letra jogada
    jogar(letra){
      this.letras.push(letra)
      this.verificaErros(letra)
    },
    // Validar erros
    verificaErros(letra){
      if(this.palavra.toLowerCase().indexOf(letra.toLowerCase()) >= 0){
        return this.verificaAcertos()
      }

      this.erros++

      if(this.erros === 6){
        this.etapa = 'enforcado'
      }
    },
    verificaAcertos(){
      let letrasUnicas = [...new Set(this.palavra.split(''))]
      if(letrasUnicas.length == (this.letras.length - this.erros)){
        this.etapa = 'ganhador'
      }
    },
    jogarNovamente(){
      this.palavra = ''
      this.dica = ''
      this.erros = 0
      this.letras = []
      this.tela = 'inicio'
      this.etapa = 'palavra'
    }

  }
}
</script>

<style>
#app{
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
