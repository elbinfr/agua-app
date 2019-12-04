<template>
  <div id="app">
    <el-container>
    <el-aside width="70px">
      <Sidebar/>
    </el-aside>
    <el-container>
      <el-header>Cabecera</el-header>
      <el-main>
        <Content/>
      </el-main>
    </el-container>
  </el-container>
  </div>
</template>

<script>
import Sidebar from './components/Sidebar'
import Content from './components/Content'

export default {
  name: 'app',
  components: {
    Sidebar,
    Content
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
