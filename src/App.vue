<template>
  <router-view />
</template>

<script>
import {defineComponent, onMounted} from 'vue'

export default defineComponent({
  name: 'App',
  mounted() {
    console.log(this.hello);
  }
})
</script>
