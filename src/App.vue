<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="warning">
      <p class="custom-block-title">Vue CLI 现已处于维护模式!</p>
      <p>现在官方推荐使用 <a href="https://github.com/vuejs/create-vue" target="_blank" rel="noopener noreferrer">create-vue</a> 来创建基于 <a href="https://vitejs.dev/" target="_blank" rel="noopener noreferrer">Vite</a> 的新项目。另外请参考 <a href="https://cn.vuejs.org/guide/scaling-up/tooling.html" target="_blank" rel="noopener noreferrer">Vue 3 工具链指南</a> 以了解最新的工具推荐。</p>
    </div>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data(){
    return {
      num:"1", 
    }
  },
  components: {
    HelloWorld
  },
  created(){
        
  } 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.warning{
  border-color: #e7c000;
  color: #6b5900;
  background-color: #ffe5644d;
}
</style>
