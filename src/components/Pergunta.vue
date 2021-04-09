<template>
  <div>
    <acao-pergunta v-if="exibirPergunta" :pergunta="perguntaAtual" @responder="acaoResposta" />    

    <resultado-pergunta v-if="!exibirPergunta" :textoResultado="textoResultado" :exibirUltimoResultado="exibirUltimoResultado" :totalAcertos="totalAcertos" @continuar="continuar" />
  </div>
</template>

<script>
import AcaoPergunta from './AcaoPergunta.vue';
import ResultadoPergunta from './ResultadoPergunta.vue';

export default {
  components: { AcaoPergunta, ResultadoPergunta },
  name: 'Pergunta',
  props: ['perguntas', 'exibirDialogo'],
  data() {
    return {
      numeroPergunta: 1,
      totalAcertos: 0,
      exibirPergunta: true,
      textoResultado: "",
    };    
  },
  created: function() {
    console.log('Total perguntas', this.totalPerguntas, this.perguntas);
  },
  computed: {
    totalPerguntas: function() {
      return this.perguntas.length;
    },
    perguntaAtual: function() {
      if (this.numeroPergunta > this.totalPerguntas) return null;      
      return this.perguntas[this.numeroPergunta - 1];
    },
    exibirUltimoResultado: function() {
      return this.numeroPergunta == this.perguntas.length;
    }
  },
  methods: {
    // Dispara a ação de responder a pergunta
    acaoResposta(acertou) {      
      if (acertou) {
        this.totalAcertos++;
        this.textoResultado = this.perguntaAtual.textoCerto ?? "Parabéns, você acertou!";
      } else {
        this.textoResultado = this.perguntaAtual.textoErrado ?? "Que pena, tenta na próxima...";
      }

      console.log("Total acertos", this.totalAcertos);
      this.exibirPergunta = false;
    },

    //Ação ao clicar no botão proxima pergunta
    continuar() {
      if (this.exibirUltimoResultado) {
        // Avisar ao nunuStudio que o jogo foi finalizado
        //Enviar de volta ao nunustudio o resultado/pontuação
        console.log("Enviar resultado nunuStudio", this.totalAcertos);
        this.$emit("update:exibirDialogo", false);
        return;
      }

      console.log("Continuar");
      this.numeroPergunta++;
      this.exibirPergunta = true;
    }
  }
}
</script>