<template>
    <v-layout>
        <v-flex xs8 offset-xs2>
            <v-form>
                <v-card>
                    <v-card-title class="title">
                        Deltacon Lottery
                    </v-card-title>
                    <v-divider></v-divider>
                    <v-card-text>
                        <v-textarea label="Jogos" hint="Informe os números separados por - (informe 3 jogos por linha), divididos por | " v-model="numeros"></v-textarea>
                    </v-card-text>
                    <v-divider></v-divider>
                    <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn flat @click="gerarJogos()">Gerar</v-btn>
                    </v-card-actions>
                </v-card>
                <template v-if="jogos.length > 0">
                    <br/>
                    <v-card>
                        <v-card-title class="title">
                            Jogos
                        </v-card-title>
                        <v-divider></v-divider>
                        <v-card-text class="text-xs-left">
                            <v-data-table :headers="[ { text: '#', value: 'quantidade' }, { text: 'Números', value: 'hash' }, { text: 'Soma', value: 'soma', align: 'center' }, { text: 'Duplicado', value: 'repetido', align: 'right' } ]" :items="jogos" class="elevation-1">
                                <template v-slot:items="props">
                                    <td class="text-xs-left">{{ props.item.quantidade }}</td>
                                    <td class="text-xs-left">{{ props.item.hash }}</td>
                                    <td class="text-xs-center">{{ props.item.soma }}</td>
                                    <td class="text-xs-right">{{ props.item.repetido ? 'Sim' : 'Não' }}</td>
                                </template>
                            </v-data-table>
                        </v-card-text>
                        <v-divider></v-divider>
                    </v-card>
                    <br/>
                    <v-card>
                        <v-card-title class="title">
                            Ranking
                        </v-card-title>
                        <v-divider></v-divider>
                        <v-card-text>
                            <v-data-table :headers="[ { text: 'Número', value: 'numero' }, { text: 'Ocorrências', value: 'ocorrencias', align: 'right' } ]" :items="ranking" class="elevation-1">
                                <template v-slot:items="props">
                                    <td class="text-xs-left">{{ props.item.numero }}</td>
                                    <td class="text-xs-right">{{ props.item.ocorrencias}}</td>
                                </template>
                            </v-data-table>
                        </v-card-text>
                    </v-card>
                </template>
            </v-form>
        </v-flex>
    </v-layout>
</template>

<script>
export default {
    data() {
        return {
            numeros: '06,19,29,33,43,48\n04,09,29,30,38,44\n12,29,38,44,53,57\n07,08,23,33,48,54\n04,05,13,34,38,49\n01,14,23,47,51,58\n14,18,41,43,38,11\n02,09,32,40,51,59\n03,28,07,35,21,59\n45,60,24,58,15,32\n53,39,28,15,42,35\n28,06,19,29,55,46\n01,10,11,17,24,36\n02,05,09,12,19,31\n08,14,38,40,44,47\n03,30,06,19,25,02\n07,33,28,10,09,27\n05,01,18,14,09,13\n08,12,16,28,55,60\n04,05,21,33,59,60\n11,27,30,41,43,50\n07,11,24,32,38,41\n05,07,13,17,21,32\n06,09,12,21,23,30\n09,14,26,33,49,54\n08,12,32,39,44,56\n06,15,32,36,44,58\n01,05,12,20,33,40\n07,13,15,22,45,48\n08,10,16,24,35,37\n02,13,25,29,34,48\n04,12,26,40,45,52\n03,11,36,30,46,51\n10,20,30,38,39,52\n04,07,19,16,50,46\n14,27,09,05,19,02\n29,12,13,05,17,10\n29,12,13,05,08,02\n25,05,13,05,29,12\n01,09,17,26,55,58\n04,08,12,16,26,32\n01,07,19,47,51,53\n03,07,13,22,41,56\n05,09,12,19,32,48\n02,07,10,23,33,50',
            jogos: [],
            ranking: [],
            resultados: ''
        }
    },

    methods: {
        gerarJogos() {
            // Parse temporário das apostas
            let apostas = this.numeros.split('\n')

            // Gera os jogos
            apostas.forEach((aposta) => {
                // Jogo
                let jogo = { numeros: [], auto: false }

                // Organiza os números e gera o ranking
                aposta.split(',').forEach((a, j) => {
                    let n = parseInt(a)
                    this.ranking[n-1].ocorrencias++
                    this._.sortBy(jogo.numeros)
                    jogo.numeros.push(n)
                })

                // Calcula as métricas
                this.calcularMetricas(jogo)

                // Adiciona
                this.jogos.push(jogo)
            })

            // Verifica se repetiu algum jogo
            this.verificaJogosRepetidos()
        },

        calcularMetricas(jogo) {
            // Quantidade de numeros
            jogo.quantidade = jogo.numeros.length

            // Gera dados adicionais do jogo
            jogo.hash = this._.join(this._.map(jogo.numeros, (v) => { return v > 9 ? `${v}` : `0${v}` }), ' - ')

            // Soma tudo
            jogo.soma = this._.sum(jogo.numeros)
        },

        gerarApostasNovas() {
            // Gera 30 jogos (15 mais jogados, 15 menos jogados)
            let n = 15

            // Gera os jogos (mais jogados)
            for (let i = 0; i < n; i++) {
                // Cria o jogo
                let jogo = { numeros: [], auto: true }

                // Obtém os números do jogo

            }
        },

        verificaJogosRepetidos() {
            // Verifica
            this.jogos.forEach((j, i) => {
                let match = this._.filter(this.jogos, { 'hash': j.hash }).length > 1
                j.repetido = match
            })
        }
    },
    
    created() {
        for(let i = 0; i < 60; i++) {
            this.ranking[i] = { numero: i+1, ocorrencias: 0 }
        }
    }
}
</script>

<style>

</style>
