<template>
  <div class="about">
    <h1>This is an about page</h1>

    <HelloWorld msg="Hello George" /> 
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'
export default {
  name: 'about',
  components: {
    HelloWorld
  }
}
</script>
