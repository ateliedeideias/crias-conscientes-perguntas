<template>
  <div id="app">    
    <md-dialog :md-active.sync="exibirPerguntas" :md-fullscreen="false">
      <div class="pergunta">
        <Pergunta :perguntas="perguntasAleatorias" :exibirDialogo.sync="exibirPerguntas" />
      </div>
    </md-dialog>
  </div>
</template>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
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
  background-color: #FF0000 !important;
}
</style>

<script>
import Pergunta from './components/Pergunta.vue'
import jsonPerguntas from './assets/dados/perguntas.json';

export default {
  name: 'App',
  components: {
    Pergunta
  },
  data() {
    return {
      perguntas: jsonPerguntas,
      exibirPerguntas: false,
      nivel: 1,      
      origem: "hotdog",
      totalPerguntas: 3,
    }
  },
  computed: {
    perguntasValidas: function() {
      return this.perguntas.filter(p => p.nivel === this.nivel && p.origem === this.origem);
    },
    perguntasAleatorias: function() {
      const controlePerguntas = [];
      const perguntas = [];
      const maximo = this.perguntasValidas.length;

      // Repetir enquanto não tiver total de perguntas necessárias (controleada pela variável this.totalPerguntas)
      while(perguntas.length < this.totalPerguntas) {
        const aleatorio = Math.random();        
        //Número da posição da pergunta
        const numeroFinal = Math.floor(aleatorio * maximo);

        if (controlePerguntas.indexOf(numeroFinal) === -1) {
          perguntas.push(this.perguntasValidas[numeroFinal]);
          controlePerguntas.push(numeroFinal);
        }        
      }

      return perguntas;
    }
  },
  created: function () {
    this.iniciarPerguntas(1, 'hotdog');
  },
  methods: {
    iniciarPerguntas(nivel, origem) {
      console.log(`Iniciar perguntas para o nível ${nivel} da origem ${origem}`);
      this.exibirPerguntas = true;
    }
  },
}
</script>
