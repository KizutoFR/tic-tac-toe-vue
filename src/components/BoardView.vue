<template>
    <div class="container">
        <h2 v-if="winner">Winner: {{winner}} 😎</h2>
        <h2 v-else>Players Move : {{player}}</h2>
        <button @click="reset" class="btn btn-success mb-3">Reset Game</button>

        <div class="game">
            <div v-for="x in 3" :key="x" class="row">
                <button v-for="y in 3" :key="y" class="square" @click="move(x,y)" :id="`${x}${y}`">
                    {{ squares[x-1][y-1] }} 
                </button>      
            </div>
        </div>
        
        <h2 class="mt-5">History</h2>
        <button @click="resetHistory" class="btn btn-success mb-3">Reset History</button>
        <div v-for="(game,idx) in history" :key="idx">
            Game {{idx + 1}}: {{ game }} won
        </div>
        <transition name="fade">
            <NotificationView v-show="notification" />
        </transition>
        <PopupView :status="status" @reset="reset" />
    </div>

</template>

<script lang="ts">
import { defineComponent, computed, ref, watch, onMounted } from 'vue'
import NotificationView from "@/components/NotificationView.vue";
import PopupView from '@/components/PopupView.vue'

export default defineComponent({
    components:{NotificationView,PopupView},
    setup() {
        const calculateWinner = (squares:string[]) => {
            const lines = [
                [0,1,2],
                [3,4,5],
                [6,7,8],
                [0,3,6],
                [1,4,7],
                [2,5,8],
                [0,4,8],
                [2,4,6]
            ]

            for (let i = 0; i < lines.length; i++) {
                const [a, b, c] = lines[i]
                if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]){
                    document.getElementById(correspondence(a))?.classList.add('active')
                    document.getElementById(correspondence(b))?.classList.add('active')
                    document.getElementById(correspondence(c))?.classList.add('active')                    
                    return squares[a];
                }
            }

            return null
        }

        const correspondence = (i:number) => {
            let id:string;
            switch (i) {
                case 0:
                    id='11';
                    break;
                case 1:
                    id='12';
                    break;
                case 2:
                    id='13';
                    break;
                case 3:
                    id='21';
                    break;
                case 4:
                    id='22';
                    break;
                case 5:
                    id='23';
                    break;
                case 6:
                    id='31';
                    break;
                case 7:
                    id='32';
                    break;
                case 8:
                    id='33';
                    break;
                
                default:
                    id='0'
                    break;
            }
            return id
        }

        const player = ref('X')
        const squares = ref([
            ['','',''],
            ['','',''],
            ['','','']
        ])

        const winner = computed(()=> calculateWinner(squares.value.flat()))
        const playedMove = ref(0)
        const notification = ref(false)
        const move = (x: number, y: number) => {
            if (winner.value) return
            if (squares.value[x-1][y-1] === 'X' || squares.value[x-1][y-1] === 'O' ) {
                console.log("you can't move here");
                notification.value = true
                setTimeout(()=> (notification.value = false), 1000)
                return
            }
            squares.value[x-1][y-1] = player.value
            playedMove.value +=1
            if(player.value === 'X') {
                player.value = 'O'
            } else {
                player.value = 'X'
            }
            
        }

        const reset = () =>{
            player.value = 'X'
            playedMove.value = 0
            squares.value = [
                ['','',''],
                ['','',''],
                ['','','']
            ]
            while( document.getElementsByClassName('active').length){
                document.getElementsByClassName('active')[0].classList.remove('active')
            }
        }

        const history = ref<string[]>([])

        watch(winner,(current,previous) => {
            if (current && ! previous){
                history.value.push(current)
                localStorage.setItem('history',JSON.stringify(history.value))
            }
            
        })

        onMounted(()=>{
            if( localStorage.getItem('history') != null){     
                // eslint-disable-next-line @typescript-eslint/no-non-null-assertion
                history.value=(JSON.parse(localStorage.getItem('history')!))         
            }
        })

        const resetHistory = () => {
            localStorage.removeItem('history');
            window.location.reload();
        }

        const status = computed(()=>{
            if(winner.value === 'X') return 'X'
            if(winner.value === 'O') return 'O'
            if(playedMove.value === 9) return 'draw';
            console.log(playedMove.value);
            
            return ''
        })

        return { winner, player, squares, notification, history, status, move, reset , resetHistory}
    },
})
</script>

<style scoped>
    .game{
        margin: 50px auto;
        width: fit-content;
    }
    .square {
      background: #fff;
      border: 1px solid #999;
      float: left;
      font-size: 70px;
      font-weight: bold;
      line-height: 34px;
      height: 100px;
      margin-right: -1px;
      margin-top: -1px;
      padding: 0;
      text-align: center;
      width: 100px;
    }
    .active{
        background-color: #eafa12;
    }
    .fade-enter-active, .fade-leave-active {
      transition: opacity .5s
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
      opacity: 0
    }
    </style>
