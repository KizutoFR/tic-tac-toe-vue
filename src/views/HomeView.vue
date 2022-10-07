<template>
  <div class="home">
    <BoardView/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";

import BoardView from '@/components/BoardView.vue'

export default defineComponent ({
  props:{},
  components: {BoardView},
  setup() {

    return { }
  },
}

) 
</script>