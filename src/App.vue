<template>
  <div id="app">      
    <md-dialog :md-active.sync="exibirPerguntas" :md-fullscreen="false">
      <div class="pergunta">
          <Pergunta :perguntas="perguntasAleatorias" @esconderDialogo="esconderDialogo" />
      </div>
    </md-dialog>
  </div>
</template>

<script>
import Pergunta from './components/Pergunta.vue';
import jsonPerguntas from './assets/dados/perguntas.json';

export default {
  name: 'App',
  components: {
    Pergunta
  },
  data() {
    return {
      perguntas: jsonPerguntas,
      nivel: 2,
      origem: "campodefutebol",
      totalPerguntas: 3,
      exibirPerguntas: false,
    }
  },
  computed: {
    perguntasValidas: function() {
      return this.perguntas.filter(p => p.nivel===this.nivel&&p.origem===this.origem);
    },
    perguntasAleatorias: function() {
      const controlePerguntas = [];
      const perguntas = [];
      const maximo = this.perguntasValidas.length;
      const maximoPerguntas = this.perguntasValidas.length > this.totalPerguntas ? this.totalPerguntas : this.perguntasValidas.length;
      console.log('Maximo perguntas', maximoPerguntas);

      while(perguntas.length < maximoPerguntas) {
        const aleatorio = Math.random();
        const numeroFinal = Math.floor(aleatorio * maximo);
        console.log('Numero final', numeroFinal);

        if (controlePerguntas.indexOf(numeroFinal) === -1){
          console.log("Aleatório", numeroFinal);
          perguntas.push(this.perguntasValidas[numeroFinal]);
          controlePerguntas.push(numeroFinal);
        }
      }
      return perguntas;
    } 
  },
  created: function() {
    this.iniciarPerguntas(1, 'lixeira');
  },
  methods: {
    iniciarPerguntas(nivel, origem) {
      console.log(`Iniciar perguntas para o nivel ${nivel} da origem ${origem}`);
      this.exibirPerguntas  = true;
    },
    esconderDialogo() {
      console.log("Esconder dialogo");
      this.exibirPerguntas = false;
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #33f078;
  margin-top: 40px;
}
.pergunta {
  background-color: #00d0c2;
  padding: 10px;
  border-color: #000 !important;    
  border-width: 3px !important; 
  border-bottom-width: 6px !important;   
  border-style: solid !important;
  border-radius: 10px !important;
}
.md-theme-default {
  background-color: transparent !important;
}
</style>
