<template>
  <Table />
</template>

<script>
import Table from './components/Table.vue'

export default {
  name: 'App',
  components: {
    Table
  }
}
</script>

<style lang="scss">
*{
  box-sizing: border-box;
  scrollbar-width: thin;
  scrollbar-color: #303852 #EDEDEB;
}

*::-webkit-scrollbar {
  width: 6px;     
  height: 6px; 
}

*::-webkit-scrollbar-track {
  background: #EDEDEB;
  border-radius: 3px;    
}

*::-webkit-scrollbar-thumb {
  background: rgba($color: #303852, $alpha: 0.6);
  border-radius: 8px;
}

p {
  margin: 0;
}
html{
  overflow-x: hidden;
}
#app {
  padding: 40px 30px;
}
</style>
