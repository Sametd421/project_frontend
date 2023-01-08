<template>
  <div class="home">
    <HelloWorld style="color: white" msg="Herzlich Willkommen!"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'HomeView',
  components: {
    HelloWorld
  }
}
</script>
