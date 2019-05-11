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
            numeros: '06,19,29,33,43,48\n04,09,29,30,38,44\n12,29,38,44,53,57\n30,27,24,54,55,10\n05,11,27,58,44,13\n24,30,54,53,41,12\n07,08,23,33,48,54\n04,05,13,34,38,49\n01,14,23,47,51,58\n06,07,38,39,40,56\n36,39,43,51,52,59\n15,18,19,32,38,56\n08,10,12,15,17,60\n01,25,05,58,19,20\n29,12,56,11,53,15\n14,18,41,43,38,11\n02,09,32,40,51,59\n03,28,07,35,21,59\n16,28,43,54,58,60\n12,21,42,48,54,58\n09,13,22,44,39,41\n11,13,23,34,01,35\n07,47,59,02,10,35\n13,27,21,17,10,43\n45,60,24,58,15,32\n53,39,28,15,42,35\n28,06,19,29,55,46\n01,10,11,17,24,36\n02,05,09,12,19,31\n08,14,38,40,44,47\n04,30,19,14,12,60\n16,10,35,50,33,20\n03,24,32,15,26,11\n03,30,06,19,25,02\n07,33,28,10,09,27\n05,01,18,14,09,13\n03,14,27,44,58,60\n07,13,27,34,39,52\n04,17,33,37,42,50\n08,12,16,28,55,60\n04,05,21,33,59,60\n11,27,30,41,43,50\n07,17,18,20,32,41\n04,06,11,17,22,33\n06,08,13,19,24,35\n07,11,24,32,38,41\n05,07,13,17,21,32\n06,09,12,21,23,30\n09,14,26,33,49,54\n08,12,32,39,44,56\n06,15,32,36,44,58\n02,15,32,37,46,54\n08,11,12,16,17,44\n04,08,15,16,23,42\n01,17,13,28,36,57\n03,09,16,56,29,37\n59,60,29,07,09,18\n10,18,32,33,51,54\n22,24,28,42,48,50\n02,10,31,42,43,50\n01,05,12,20,33,40\n07,13,15,22,45,48\n08,10,16,24,35,37\n02,11,31,45,48,51\n09,11,31,52,55,56\n11,15,31,24,38,47\n04,35,37,38,48,60\n05,13,16,25,26,41\n07,16,29,34,37,51\n02,13,25,29,34,48\n04,12,26,40,45,52\n03,11,36,30,46,51\n10,28,30,38,39,52\n04,07,14,16,50,46\n14,27,09,05,19,02\n29,12,13,05,17,10\n29,12,13,05,08,02\n01,09,17,26,55,58\n04,08,12,16,26,32\n01,07,19,47,51,53\n05,08,22,34,50,52\n23,30,41,48,51,60\n02,20,26,35,40,54\n03,07,13,22,41,56\n05,09,12,19,32,48\n02,07,10,23,33,50\n07,13,14,19,51,53\n01,09,19,54,56,57\n13,21,24,43,46,35\n25,05,13,29,12,17\n21,22,26,42,47,59\n21,25,34,46,52,54\n35,42,43,46,49,56\n26,31,39,43,49,50\n06,31,35,36,47,56\n42,45,46,50,52,56\n03,06,18,37,51,59\n20,39,43,46,52,57\n31,35,49,52,56,57\n22,25,37,43,50,51\n17,19,08,10,41,28\n13,07,12,05,09,11\n32,02,15,24,29,30\n33,38,48,53,58,04\n23,60,01,14,16,27\n44,54,06,21,35,51\n52,56,03,22,25,34\n39,43,50,26,42,45\n47,18,31,37,55,36\n40,49,59,20,46,57\n08,14,15,23,24,29\n09,14,19,23,41,53\n10,15,24,32,38,60\n07,09,11,14,23,33\n01,07,33,41,48,58\n01,05,10,11,13,27\n05,10,16,19,30,32\n07,12,14,15,16,24\n02,07,09,13,19,23\n12,19,27,28,33,53\n14,22,30,38,41,48,57\n09,07,50,55,27,19,24\n09,17,41,42,45,52,55\n08,28,17,32,29,53,54\n11,21,28,41,48,58,39\n21,25,31,06,17,23,12\n03,19,25,34,41,53,60\n28,30,13,15,47,58,05\n07,11,23,37,44,52,53\n17,19,33,45,49,58,60\n04,09,19,21,23,28,36\n03,08,26,45,49,53,56\n05,15,18,22,34,41,53\n08,11,24,31,47,52,58\n02,17,25,28,30,42,48\n01,06,10,22,27,39,56\n13,23,34,44,47,49,53\n10,53,05,45,25,38,33\n23,27,41,43,49,52,57',
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
