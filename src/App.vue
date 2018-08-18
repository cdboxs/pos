<template>
  <div id="app">
    <el-container>
      <el-header><leftNav></leftNav></el-header>
      <el-main>
        <home></home>
      </el-main>
    </el-container>
   
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav';
import home from '@/components/page/home';
export default {
  name: 'App',
  components:{leftNav,home}
}
</script>

<style>
body,html,#app {
  font-family: "微软雅黑", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding:0;
  margin:0;
  height:100%;
}
.el-header,.el-main{
  padding:0;
  margin:0;
}
a{
  text-decoration: none;
}
li{
  list-style: none;
}
</style>
