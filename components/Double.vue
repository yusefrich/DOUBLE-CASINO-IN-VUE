<template>
    <div class="container my-5">
        <h5>
            Seu saldo na carteira: <span class="text-primary"> {{ `R$ ${wallet},00 ` }}</span>
        </h5>
        <div>
            <button class="btn btn-danger border" @click="corSelecionada = 'vermelho'">2x</button>
            <button class="btn btn-light border" @click="corSelecionada = 'branco'">14x</button>
            <button class="btn btn-dark border" @click="corSelecionada = 'preto'">2x</button>
        </div>
        <div class="my-3">
            <label for="aposta">Valor da Aposta:</label>
            <input type="number" id="aposta" v-model.number="valorAposta" :max="wallet" />
            <button class="btn btn-primary" @click="iniciarJogo()"
                :disabled="indiceEtapa !== 0 || !corSelecionada || !valorAposta || valorAposta > wallet">Apostar</button>
        </div>
        <span class="timer-count">
            {{ contador.toFixed(1) }} {{ etapas[indiceEtapa] }}
        </span>
        <div v-if="indiceEtapa === 2">
            <p>O número gerado foi {{ numeroGerado }}</p>
            <p v-if="resultado === 'Parabéns! Você ganhou!'">Você ganhou!</p>
            <p v-if="resultado === 'Que pena! Tente novamente.'">Você perdeu!</p>
        </div>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            contador: 10,
            etapas: ['Iniciar', 'Girando', 'Apresentações'],
            indiceEtapa: 0,
            corSelecionada: null,
            valorAposta: null,
            resultado: null,
            numeroGerado: null,
            wallet: 500
        }
    },
    mounted() {
        setInterval(() => {
            this.contador -= 0.1
            if (this.contador < 0.1) {
                this.indiceEtapa = (this.indiceEtapa + 1) % this.etapas.length
                this.contador = 10
                if (this.indiceEtapa === 2) {
                    this.gerarNumero()
                    this.avaliarResultado()
                }
            }
        }, 100)
    },
    methods: {
        iniciarJogo() {
            if (this.valorAposta > this.wallet) {
                alert('Valor da aposta é maior do que a carteira')
                return
            }
            if (!this.corSelecionada || !this.valorAposta) {
                alert('Selecione uma cor e insira o valor da aposta')
                return
            }
            this.resultado = null
            this.indiceEtapa = 1
            // this.wallet -= this.valorAposta
        },
        gerarNumero() {
            this.numeroGerado = Math.floor(Math.random() * 15) + 1
        },
        avaliarResultado() {
            if (this.numeroGerado <= 7 && this.corSelecionada === "vermelho") {
                this.resultado = "Parabéns! Você ganhou!";
                this.wallet += this.valorAposta * 2;
            } else if (this.numeroGerado > 7 && this.numeroGerado <= 14 && this.corSelecionada === "preto") {
                this.resultado = "Parabéns! Você ganhou!";
                this.wallet += this.valorAposta * 2;
            } else if (this.numeroGerado === 15 && this.corSelecionada === "branco") {
                this.resultado = "Parabéns! Você ganhou!";
                this.wallet += this.valorAposta * 14;
            } else {
                this.resultado = "Que pena! Tente novamente.";
                this.wallet -= this.valorAposta;
            }
            this.indiceEtapa = 2;
        }
    }
}
</script>