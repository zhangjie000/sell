<template>
  <div id="app">
    <!-- <i class="icon iconfont icon-cartfill"></i>
    <i class="icon iconfont icon-settings"></i>
    <i class="icon iconfont icon-shop"></i>
    <i class="icon iconfont icon-pay"></i>
    <i class="icon iconfont icon-bofang"></i> -->
    <leftNav></leftNav>
    <div class="main">
      <router-view></router-view>
    </div>

  </div>
</template>

<script>
import leftNav from '@/components/common/letNav'
export default {
  name: 'app',
  components:{leftNav}
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#app i{
  font-size:30px;
}

.main{
  float:left;
  width:calc(100% - 80px);
  background-color: #eff2f7;
  height:100%;
}
</style>
