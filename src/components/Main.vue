<template>
    
    <div class="grid">
        <div class="classificacao" style="display: flex">   
            <div style="float:left; margin-right: 50px;" >
                <table style="display: inline">
                    <thead>
                    <th>Rodadas</th>
                    <th v-for="n in numRodadas" v-bind:key = n> {{n}}º</th>
                    </thead>
                    <tbody>
                        <tr v-for="(participante, index) in participantes" v-bind:key="index" >
                            <td>
                                {{participante.nome}}
                            </td>
                            <td v-for="(pontuacao, index) in participante.acumulado" v-bind:key="index" > {{pontuacao}} </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            
            <div class="" >
                <table style="display: inline">
                    <thead>
                        <th>Classificação</th>
                        <th>Pontos</th>
                        <th>Média</th>
                    </thead>
                    <tbody>
                        <tr v-for="(participante, index) in classificacaoOrdenada()" v-bind:key="index" >
                            <td>
                                {{participante.nome}}
                            </td>
                            <td>
                                {{participante.total}}
                            </td>
                            <td>
                                {{participante.pontuacao.length > 0 ? (participante.total / participante.pontuacao.length).toFixed(1) : ''}}
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            


        </div>

            
        
    </div>
    <br>
    <div class="grid">
        <div class="classificacao" style="display: flex;">
            
            <div style="float:left; margin-right: 20px;">
                <table style="display: inline">
                    <thead>
                    <th>Rodadas</th>
                    <th v-for="n in numRodadas" v-bind:key = n> {{n}}º</th>
                    </thead>
                    <tbody>
                        <tr v-for="(participante, index) in participantes" v-bind:key="index" >
                            <td>
                                {{participante.nome}}
                            </td>
                            <td v-for="(posicao, index) in participante.posicaoRodada" v-bind:key="index" > {{posicao ? posicao+"º":""}} </td>
                        </tr>
                    </tbody>
                </table>
            </div>

             <div class="dado-container">
                <button class="button" v-if="!mostrarBotaoReiniciar" v-on:click="rolarJogada">Rolar dado</button>
                <button class="button button-reset" v-if="mostrarBotaoReiniciar" v-on:click="reiniciarCampeonato">Reiniciar</button>
                <br/>
                
                <div class="dado" v-bind:style="{ backgroundImage: bgdado1 }"></div>
                <div class="dado" v-bind:style="{ backgroundImage: bgdado2 }"></div>
            </div>
        </div>


    </div>

   
</template>

<script>
export default {
    name: 'Form',
    components: {
        
    },
    data() {
        return {
            numRodadas: 10,
            rodadaAtual: 1,
            participanteAtual: 0,
            dado1: 0,
            dado2: 0,
            mostrarBotaoReiniciar: false,
            bgdado1: '',
            bgdado2: '',
            participantes: [
                    {"id": 1, "nome": "Naruto", "pontuacao": [], "total": 0, "acumulado": [], "posicaoRodada": []},
                    {"id": 2, "nome": "Sasuke", "pontuacao": [],"total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 3, "nome": "Kakashi", "pontuacao": [], "total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 4, "nome": "Gaara", "pontuacao": [],"total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 5, "nome": "Rock Lee", "pontuacao": [], "total": 0,  "acumulado": [],"posicaoRodada": []},
                    {"id": 6, "nome": "Sakura", "pontuacao": [], "total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 7, "nome": "Shikamaru", "pontuacao": [], "total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 8, "nome": "Neji", "pontuacao": [], "total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 9, "nome": "Jiraya", "pontuacao": [], "total": 0, "acumulado": [],"posicaoRodada": []},
                    {"id": 10, "nome": "Orochimaru", "pontuacao": [], "total": 0, "acumulado": [],"posicaoRodada": []},
            ]
        }
    },
    mounted(){
        for(let p of this.participantes){
            p.acumulado = new Array(this.numRodadas);
            p.posicaoRodada = new Array(this.numRodadas);
        }
    },
    methods: {
        rolarJogada(){
            if(this.rodadaAtual <= this.numRodadas){
                this.dado1 = Math.floor(Math.random() * 6) + 1;
                this.dado2 = Math.floor(Math.random() * 6) + 1;
                this.bgdado1 = 'url(\'src/assets/d'+this.dado1+'.svg\')';
                this.bgdado2 = 'url(\'src/assets/d'+this.dado2+'.svg\')';
                var somaDados = this.dado1+this.dado2;
                var participanteObj = this.participantes[this.participanteAtual];
                participanteObj.pontuacao.push(somaDados);
                participanteObj.total += somaDados;
                participanteObj.acumulado[this.rodadaAtual - 1] = (participanteObj.total);
                this.participanteAtual++;
                if(this.participanteAtual >= this.participantes.length){
                    let participantesOrdenados = this.classificacaoOrdenada();
                    for(let p of this.participantes){
                        p.posicaoRodada[this.rodadaAtual - 1] = participantesOrdenados.findIndex(po => po.nome == p.nome) + 1;
                    }
                    this.participanteAtual = 0;
                    this.rodadaAtual++;
                    if(this.rodadaAtual > this.numRodadas){
                        this.mostrarBotaoReiniciar = true;
                    }
                }
            }
        },
        classificacaoOrdenada(){
            return this.participantes.concat().sort(function(a, b){
                if(a.total == b.total){
                    var countRodadasA = a.pontuacao.filter(x => x != "").length;
                    var countRodadasB = b.pontuacao.filter(x => x != "").length;
                    if(countRodadasA == countRodadasB){
                        while(countRodadasA >= 0 && countRodadasB >= 0){
                            if(b.pontuacao[countRodadasB] != a.pontuacao[countRodadasA])
                                return b.pontuacao[countRodadasB] - a.pontuacao[countRodadasA];
                            countRodadasA--;
                            countRodadasB--;
                        }
                    }else{
                        return countRodadasA - countRodadasB;
                    }
                }
                return b.total - a.total
            });
        },
        reiniciarCampeonato(){
            this.rodadaAtual = 1;
            this.participanteAtual = 0;
            this.mostrarBotaoReiniciar = false;
            for(let p of this.participantes){
                p.acumulado = new Array(this.numRodadas);
                p.posicaoRodada = new Array(this.numRodadas);
                p.pontuacao = [];
                p.total = 0;
            }
        }
    }
}

</script>
<style scoped>
    .dado-container {
        padding-top: 50px;
    }
    .dado{
        display: inline-block;
        font-size: 60px;
        text-align: center;
        padding: 60px;
        border: 2px solid black; 
        margin: 10px 10px;
        vertical-align: middle;
        line-height: 45px;
    }

    .dado-girando{
        display: inline-block;
        font-size: 60px;
        text-align: center;
        width: 170px;
        height: 170px;
        border: 2px solid black; 
        margin: 10px 0px;
        vertical-align: middle;
        line-height: 45px;
        background: red;
    }

    table {
    border-collapse: collapse;
    width: 100%;
    }

    table.tabela-divisoes{
        width: 150px;
    }

    th, td {
    text-align: left;
    padding: 8px;
    }

    td{
        min-width: 27px;
    }

    tr:nth-child(even){background-color: #f2f2f2}

    th {
        background-color: #4CAF50;
        color: white;
    }

    .button {
        display: inline-block;
        padding: 15px 25px;
        font-size: 24px;
        cursor: pointer;
        text-align: center;
        text-decoration: none;
        outline: none;
        color: #fff;
        border: none;
        border-radius: 15px;
        box-shadow: 0 9px #999;
        background-color: #4CAF50;
    }

    .button:hover {background-color: #3e8e41}

    .button:active {
        background-color: #3e8e41;
        box-shadow: 0 5px #666;
        transform: translateY(4px);
    }

    .button-reset:hover {background-color: #c72c41}

    .button-reset{
        background-color: #ee4540;
    }

    .button-reset:active{
        background-color: #c72c41;
    }
</style>