<template>
    <div id="board">
        <div 
            class="card virado" 
            v-for="foto in fotosJogo" 
            v-bind:key="foto.uid"
            :class="foto.key % 2 == 0 ? 'giraE' : 'giraD'"
            v-on:click="vira(foto)"
        >
            <img :src="foto.status ? foto.url : cartaVirada" />
        </div>    
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
            cartaCompara: false,
            fotosJogo: [],
            cartaVirada: 'https://cdn.paciencia.co/artwork-v133/cardbacks1/back32.png',
            fotos: [
                { 
                "key": 1,
                "url": "https://cdn.pixabay.com/photo/2016/12/05/11/39/fox-1883658_960_720.jpg",
                "status": false,
                },
                { 
                "key": 2,
                "url": "https://cdn.pixabay.com/photo/2012/06/19/10/32/owl-50267_960_720.jpg",
                "status": false,
                },
                { 
                "key": 3,
                "url": "https://cdn.pixabay.com/photo/2017/03/13/10/25/hummingbird-2139279_960_720.jpg",
                "status": false,
                },
                { 
                "key": 4,
                "url": "https://cdn.pixabay.com/photo/2015/11/16/16/28/bird-1045954_960_720.jpg",
                "status": false,
                },
                { 
                "key": 5,
                "url": "https://cdn.pixabay.com/photo/2016/01/08/11/57/butterfly-1127666_960_720.jpg",
                "status": false,
                },
                { 
                "key": 6,
                "url": "https://cdn.pixabay.com/photo/2013/06/30/18/56/butterfly-142506_960_720.jpg",
                "status": false,
                },
                { 
                "key": 7,
                "url": "https://cdn.pixabay.com/photo/2012/12/13/18/50/frog-69813_960_720.jpg",
                "status": false,
                },
                { 
                "key": 8,
                "url": "https://cdn.pixabay.com/photo/2014/11/03/17/40/leopard-515509_960_720.jpg",
                "status": false,
                },
                { 
                "key": 9,
                "url": "https://cdn.pixabay.com/photo/2014/12/22/10/04/lions-577104_960_720.jpg",
                "status": false,
                },
                { 
                "key": 10,
                "url": "https://cdn.pixabay.com/photo/2014/12/12/19/45/lion-565820_960_720.jpg",
                "status": false,
                },
                { 
                "key": 11,
                "url": "https://cdn.pixabay.com/photo/2015/07/27/19/47/turtle-863336_960_720.jpg",
                "status": false,
                },
                { 
                "key": 12,
                "url": "https://cdn.pixabay.com/photo/2020/09/21/21/24/donkey-5591272_960_720.jpg",
                "status": false,
                },
                { 
                "key": 13,
                "url": "https://cdn.pixabay.com/photo/2013/11/01/11/13/dolphin-203875_960_720.jpg",
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
        let item, novo1, novo2
        let montagem = []
        while (montagem.length < this.cartas) {
            item = getRandom(0, totalFotos)
            if (!montagem.find(el => el.key != undefined && el.key == this.fotos[item].key)) {
                novo1 = { ...this.fotos[item], uid: `1_${this.fotos[item].key}` }
                novo2 = { ...this.fotos[item], uid: `2_${this.fotos[item].key}` }
                montagem.push(novo1)
                montagem.push(novo2)
            }
        }
        this.fotosJogo = montagem.sort(() => .5 - Math.random())
    },

    methods: {
        vira: function(foto) {
            if (this.jogadas === 0) {
                foto.status = !foto.status
                this.cartaCompara = foto.uid
                console.log('vai iniciar')

                this.jogadas++;
            }
            else if (this.jogadas === 1 && foto.status === false) {
                foto.status = !foto.status
                console.log('aqui', this.cartaCompara.substr(2) )
                if (this.cartaCompara.substr(2) == foto.key) {
                    this.jogadas = 0
                    console.log('boa')
                    return
                }
                else {
                    setTimeout(() => {
                        const cartaPrimeiro = this.fotosJogo.findIndex(el => el.uid == this.cartaCompara)
                        this.fotosJogo[cartaPrimeiro].status = false
                        foto.status = false
                    }, 500)

                    console.log('perdeu')
                    this.jogadas = 0
                    return
                }
            }
            else {
                this.jogadas = 0;
                return
            }
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
    /* border: 8px solid #c1c1c1; */
    -webkit-box-shadow: 1px 0px 9px 0px rgba(50, 50, 50, 0.75);
    -moz-box-shadow:    1px 0px 9px 0px rgba(50, 50, 50, 0.75);
    box-shadow:         1px 0px 9px 0px rgba(50, 50, 50, 0.75);
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
</style>