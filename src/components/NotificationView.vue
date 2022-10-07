<template>
    <div class="notification-container" :class="show ? 'show' : ''">
        <p>You have already entered this letter</p>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    props: {
        show: {
            type: Boolean,
            default: false,
        }
    }
})
</script>

<style scoped>
.notification-container {
    background-color: rgba(0, 0, 0, 0.3);
    border-radius: 10px 10px 0 0;
    padding: 15px 20px;
    position: absolute;
    left:50%;
    transform: translateX(-50%);
    bottom: 0px;
    transition: display 0.5s ease-in-out;
  }
  
  .notification-container p {
    margin: 0;
  }
  
</style>