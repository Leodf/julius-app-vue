<template>
  <div class="painelLancamento">
    <div class="formularioLancamento">
        
        <form @submit="salvar">
            <div class="tiposLancamento">
                <input type="radio" name="tipo" id="entrada" value="entrada" v-model="tipo">
                <label for="entrada">Entrada</label>
                <input type="radio" name="tipo" id="saida" value="saida" v-model="tipo">
                <label for="saida">Saída</label>
            </div>

            <label for="valor">Valor</label>
            <input type="number" min="0" step="0.01" name="valor" id="valor" v-model.number="valor" required>
            <label for="descricao">Descrição</label>
            <input type="text" name="descricao" id="descricao" v-model="descricao" required>
            <label for="data">Data</label>
            <input type="date" name="data" id="data" v-model="data" required>

            <button>Lançar</button>
        </form>
    </div>

    <div class="areaLancamentos">
        <BlocoLancamento
            v-for="lancamento in todosLancamentos"
            v-bind:key="lancamento.id"
            :tipo="lancamento.valor > 0 ? 'entrada' : 'saida'"
            :lancamento="lancamento" 
        />
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import BlocoLancamento from './BlocoLancamento.vue'
import Lancamento from '../models/Lancamento'

export default {
    name: "PainelLancamentos",
    data: () => {
        return {
            tipo: "saida", // elimina o checked no botao radio pois fica valor padrao
            valor: undefined,
            descricao: "",
            data: "",
        }
    },
    components: {
        BlocoLancamento,
    },
    computed: mapGetters(["todosLancamentos"]),
    methods: {
        ...mapActions(["salvarLancamento"]),
        salvar(event) {
            event.preventDefault()
            if (this.tipo === 'saida') {
                this.valor *= -1
            }
            const lancamento = new Lancamento(this.valor, this.descricao, this.data)
            this.salvarLancamento(lancamento)
            this.limparFormulario()
        },
        limparFormulario() {
            this.tipo = "saida"
            this.valor = undefined
            this.descricao = ""
            this.data = ""
        },
    },
}
</script>

<style scoped>
.painelLancamento {
    width: 40%;
    padding: 20px;
}

.formularioLancamento {
    background-color: white;
    border-radius: 20px;
    padding: 20px;
    font-family: "padrao";
}

#valor,
#descricao,
#data,
button {
    display: block;
    margin-bottom: 10px;
}

#descricao {
    width: 70%;
}

#valor,
#descricao,
#data {
    height: 20px;
    font-family: "padrao";
    font-size: 100%;
}

button {
    background-color: var(--cor-destaque);
    border: none;
    outline: none;
    border-radius: 5px;
    padding: 10px 20px;
    color: white;
    font-family: "negrito";
    font-size: 100%;
}

.tiposLancamento {
    margin-bottom: 10px;
    font-family: "negrito";
}

.tiposLancamento label {
    margin-right: 20px;
}

.tiposLancamento label:first-of-type {
    color: #22a7f0;
}

.tiposLancamento label:last-of-type {
    color: red;
}

.areaLancamentos {
    margin-top: 30px;
}

</style>