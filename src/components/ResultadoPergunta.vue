<template>
  <div>
    <b>{{textoResultado}}</b><br /><br />
    
    <div v-if="exibirUltimoResultado">
      <span v-if="totalAcertos === 3">Parabéns você acertou tudo!</span>
      <span v-if="totalAcertos === 2">Você acertou 2 respostas.</span>
      <span v-if="totalAcertos === 1">Você só acertou uma resposta.</span>
      <span v-if="totalAcertos === 0">Que pena, você não acertou nenhuma!</span>
      <br /><br />
    </div>

    <md-button v-on:click="continuar()">{{textoBotao}}</md-button>
  </div>
</template>

<script>
export default {
  name: "resultadoPergunta",
  props: [ 'textoResultado', 'exibirUltimoResultado', 'totalAcertos' ],
  computed: {
    textoBotao: function() {
      return this.exibirUltimoResultado ? "Retornar ao Jogo" : "Próxima Pergunta";
    }
  },
  methods: {
    continuar() {
      this.$emit('continuar');
    }
  }
}
</script>