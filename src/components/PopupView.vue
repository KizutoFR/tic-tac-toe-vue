<template>
    <div v-if="finalMessage" class="popup-container" id="popup-container">
    <div class="popup">
      <h2>{{ finalMessage }}</h2>
      <button @click="reset">Play Again</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'

export default defineComponent({
    props: {
        status:{
            type: String,
            default: '',
        },
    },
    setup(props, {emit}) {
        const finalMessage = computed(()=>{
            if(props.status ==='X') return 'X won! 🐱‍👤'
            if(props.status ==='O') return 'O won! 🐱‍🏍'
            if(props.status ==='draw') return 'Draw! 🤡'
            return ''
        })

        const reset = () => emit('reset') 

        return {finalMessage, reset}
    },
})
</script>

<style scoped>
.popup-container {
    background-color: rgba(0, 0, 0, 0.3);
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    /* display: none; */
    align-items: center;
    justify-content: center;
  }
  
  .popup {
    background: #2980b9;
    border-radius: 5px;
    box-shadow: 0 15px 10px 3px rgba(0, 0, 0, 0.1);
    padding: 20px;
    text-align: center;
    width: 400px;
    height: 300px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
  .popup button {
    cursor: pointer;
    background-color: #fff;
    color: #2980b9;
    border: 0;
    margin-top: 20px;
    padding: 12px 20px;
    font-size: 16px;
  }
  
  .popup button:active {
    transform: scale(0.98);
  }
  
  .popup button:focus {
    outline: 0;
  }
</style>