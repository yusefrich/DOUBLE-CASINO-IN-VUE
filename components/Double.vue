<template>
    <div>
        <div class="container my-5">
            <div>
                <span class="text-white">
                    sua escolha foi: {{ corSelecionada }}
                </span>
            </div>
            <h5 class="text-white">
                Seu saldo na carteira: <span class="text-primary"> {{ `R$ ${wallet},00 ` }}</span>
            </h5>
            <div>
                <button class="btn btn-danger border" @click="corSelecionada = 'vermelho'">2x</button>
                <button class="btn btn-light border" @click="corSelecionada = 'branco'">14x</button>
                <button class="btn btn-dark border" @click="corSelecionada = 'preto'">2x</button>
            </div>
            <div class="my-3">
                <label class="text-white" for="aposta">Valor da Aposta:</label>
                <input type="number" id="aposta" v-model.number="valorAposta" :max="wallet" />
                <button class="btn btn-primary" @click="iniciarJogo()"
                    :disabled="indiceEtapa !== 0 || !corSelecionada || !valorAposta || valorAposta > wallet">Apostar</button>
            </div>
            <div class="progress">
                <div class="progress-bar" role="progressbar" :style="{ width: `${contador * 10}%` }">
                    <span class="timer-count">
                        {{ contador.toFixed(1) }} {{ etapas[indiceEtapa] }}
                    </span>
                </div>
            </div>
            <span class="text-white fw-bold fs-5">
                Numero da roleta: {{ numeroDaRoleta }}
            </span>
            <roll :etapa="etapas[indiceEtapa]" :winnerNumber="numeroGerado" @number-roll="atualizarNumero" />
            <div v-if="indiceEtapa === 2">
                <p class="text-white">Double girou: {{ numeroGerado }}</p>
                <p class="text-white" v-if="resultado === 'Parabéns! Você ganhou!'">Você ganhou!</p>
                <p class="text-white" v-if="resultado === 'Que pena! Tente novamente.'">Você perdeu!</p>
            </div>
        </div>
    </div>
</template>
  
<script>
import Roll from './Roll.vue';

export default {
    components: {
        Roll
    },
    data() {
        return {
            contador: 10,
            etapas: ['Iniciar...', 'Girando...', 'Vencedor:'],
            indiceEtapa: 0,
            corSelecionada: null,
            valorAposta: null,
            resultado: null,
            numeroGerado: null,
            numtest: 6,
            numeroDaRoleta: '',
            wallet: 500,
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
                    this.resetGame()
                }
            }
        }, 100)
    },
    methods: {
        atualizarNumero(num) {
            this.numeroDaRoleta = num.num
        },
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
            this.numeroGerado = this.numeroDaRoleta;
        },
        resetGame() {
            this.corSelecionada = null,
            this.numeroDaRoleta = null
        },
        avaliarResultado() {
            if (this.numeroGerado <= 7 && this.corSelecionada === "vermelho" && this.corSelecionada != null && this.valorAposta <= this.wallet) {
                this.resultado = "Parabéns! Você ganhou!";
                this.wallet += this.valorAposta * 2;
            } else if (this.numeroGerado > 7 && this.numeroGerado <= 14 && this.corSelecionada === "preto" && this.corSelecionada != null && this.valorAposta <= this.wallet) {
                this.resultado = "Parabéns! Você ganhou!";
                this.wallet += this.valorAposta * 2;
            } else if (this.numeroGerado === 15 && this.corSelecionada === "branco" && this.corSelecionada != null && this.valorAposta <= this.wallet) {
                this.resultado = "Parabéns! Você ganhou!";
                this.wallet += this.valorAposta * 14;
            } else {
                if (this.corSelecionada != null && this.valorAposta <= this.wallet) {
                    this.resultado = "Que pena! Tente novamente.";
                    this.wallet -= this.valorAposta;
                }
            }
            this.indiceEtapa = 2;
        }
    }
}
</script>

<style scoped>
.progress-bar {
    background-color: #ff0037;
    transition: width 0.1s ease-in-out;
}

.timer-count {
    position: absolute;
    left: 0;
    right: 0;
    font-size: 17px;
    color: #000;
    font-weight: bold;
}
</style>