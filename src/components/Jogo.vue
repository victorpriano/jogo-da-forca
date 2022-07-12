<template>
    <div class="jogo">
        <Forca :erros="erros" />

        <Palavra :palavra="palavra" :dica="dica" :etapa="etapa" :verificarLetra="verificarLetra" />

        <Teclado v-if="etapa === 'jogo'" :letras="letras" :jogar="jogar" />

        <Final v-if="etapa != 'jogo'" :etapa="etapa" 
            :texto="etapa === 'ganhador' ? 'Parabéns! Você venceu!!! =)' : 'Você perdeu!!! =('"
            :jogarNovamente="jogarNovamente" />
    </div>
</template>

<script>
    import Forca from './Forca.vue'
    import Palavra from './Palavra.vue'
    import Teclado from './Teclado.vue'
    import Final from './Final.vue'

    export default{
        name: 'Jogo',
        props: {
            erros: Number,
            palavra: String,
            dica: String,
            verificarLetra: Function,
            etapa: String,
            letras: Array,
            jogar: Function,
            texto: String,
            jogarNovamente: Function
        },
        components: {
            Forca,
            Palavra,
            Teclado,
            Final
        }
    }
</script>

<style>
    .jogo{
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
    }
</style>