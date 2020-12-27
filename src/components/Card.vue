<template>
    <div id="board">
        <div
            class="card virado"
            v-for="foto in fotosJogo"
            v-bind:key="foto.uid"
            :class="foto.key % 2 == 0 ? 'giraE' : 'giraD'"
            v-on:click="vira(foto)"
        >
            <img :src="foto.status ? `/static/cartas/${foto.url}` : cartaVirada" />
        </div>

        <p id="status" :class="classErro">{{msg}}</p>
        <p>{{jogadas}} - {{logstatus}} | liberado {{liberadoJogada}}</p>
    </div>
</template>

<script>
export default {
    name: "Card",
    props: {
        cartas: Number,
    },

    data() {
        return {
            jogadas: 0,
            msg: '',
            logstatus: '',
            status: '',
            classErro: '',
            cartaCompara: false,
            fotosJogo: [],
            cartaVirada: '/static/carta-virada.png',
            liberadoJogada: true,
            fotos: [
                {
                "key": 1,
                "url": "1.jpg",
                "status": false,
                },
                {
                "key": 2,
                "url": "2.jpg",
                "status": false,
                },
                {
                "key": 3,
                "url": "3.jpg",
                "status": false,
                },
                {
                "key": 4,
                "url": "4.jpg",
                "status": false,
                },
                {
                "key": 5,
                "url": "5.jpg",
                "status": false,
                },
                {
                "key": 6,
                "url": "6.jpg",
                "status": false,
                },
                {
                "key": 7,
                "url": "7.jpg",
                "status": false,
                },
                {
                "key": 8,
                "url": "8.jpg",
                "status": false,
                },
                {
                "key": 9,
                "url": "9.jpg",
                "status": false,
                },
                {
                "key": 10,
                "url": "10.jpg",
                "status": false,
                },
                {
                "key": 11,
                "url": "11.jpg",
                "status": false,
                },
                {
                "key": 12,
                "url": "12.jpg",
                "status": false,
                },
                {
                "key": 13,
                "url": "13.jpg",
                "status": false,
                },
            ]
        }
    },

    created() {
        const getRandom = (min, max) => {
            return Math.floor(Math.random() * (max - min + 1)) + min
        }

        const totalFotos = this.fotos.length - 1
        let item, carta, cartaIgual
        let montagem = []
        let i = 1
        while (i <= this.cartas) {
            item = getRandom(0, totalFotos)
            if (!montagem.find(el => el.key != undefined && el.key == this.fotos[item].key)) {
                carta      = { ...this.fotos[item], uid: `1_${this.fotos[item].key}` }
                cartaIgual = { ...this.fotos[item], uid: `2_${this.fotos[item].key}` }
                montagem.push(carta)
                montagem.push(cartaIgual)

                i++;
            }
        }
        this.fotosJogo = montagem.sort(() => .5 - Math.random())
    },

    methods: {
        vira: function(foto) {

            if (this.jogadas > 1) return;

            this.logstatus      = false
            this.liberadoJogada = false

            //Primeira jogada
            if (this.jogadas === 0) {
                foto.status         = !foto.status
                this.cartaCompara   = foto.uid
                this.status         = 'inicio'

                this.jogadas++;
            }
            //Segunda jogada
            else if (this.jogadas === 1 && foto.status === false) {
                foto.status = !foto.status

                //Verifica se a segunda carta em o mesmo id da primeira
                if (this.cartaCompara.substr(2) == foto.key) {
                    this.logstatus    = true
                    setTimeout(() => {
                        this.status   = 'acerto';
                        foto.status = false
                        this.jogadas = 0
                    }, 700)
                    return
                }
                //Segunda carta errada
                else {
                    this.status   = 'erro'
                    setTimeout(() => {
                        const cartaPrimeiro = this.fotosJogo.findIndex(el => el.uid == this.cartaCompara)
                        this.fotosJogo[cartaPrimeiro].status = false
                        this.status   = '';
                        foto.status = false
                        this.jogadas = 0
                    }, 700)

                    return
                }
            }
        }
    },

    watch: {
        status(i) {
            console.log('dentro do i', i)
            this.classErro = ''
            if (i == 'erro') {
                this.msg = 'Ops! Tente novamente!'
                this.classErro = 'erro'
            }
            else if (i == 'acerto') {
                this.msg = 'Você acertou! Excelente!'
            }
            else {
                this.msg = ''
            }
            this.liberadoJogada = true
        }
    }
}
</script>

<style scoped>
#board {
    width: 90%;
    height: 90%;
    margin: 0 auto;
    padding: 50px 0;
    text-align:center;
}
.card {
    width: 20%;
    height: 200px;
    margin: 20px;
    display: inline-flex;
    cursor: pointer;
    -webkit-box-shadow: 1px 0px 9px 0px rgba(50, 50, 50, 0.75);
    -moz-box-shadow:    1px 0px 9px 0px rgba(50, 50, 50, 0.75);
    box-shadow:         1px 0px 9px 0px rgba(50, 50, 50, 0.75);
    border: 8px solid silver;
}
.card2:hover {
    border: 8px solid #000;
}
.card.giraE:hover, .card.giraD:hover {
    -webkit-transform:scale(1.2) rotate(10deg);
    -moz-transform:scale(1.2) rotate(10deg);
    -o-transform:scale(1.2) rotate(10deg);
    transform:scale(1.2) rotate(10deg);
}
.giraE {
    transform: rotate(5deg);
}
.giraD {
    transform: rotate(-5deg);
}
.card img {
    width: 100%;
    height: 100%;
}
#status {
    font-size: 2.4rem;
    font-weight: bold;
    margin-top: 70px;
}
.erro {
    color: red;
}
</style>
