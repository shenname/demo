<template>
  <el-row class="tac">
    <el-col :span="24">
      <el-menu
        default-active="dashboard"
        class="el-menu-vertical-demo"
        background-color="#324157"
        router
        @open="handleOpen"
        @close="handleClose">
        <el-menu-item :index="item.componentName" v-for="(item,index) in menu" :key="index">
          <i class="el-icon-menu"></i>
          <span slot="title">{{item.name}}</span>
        </el-menu-item>
      </el-menu>
    </el-col>
  </el-row> 
</template>

<style scoped>
.over-hide{
  overflow: hidden;
}
.el-menu-vertical-demo:not(.el-menu--collapse) {
  height: calc(100vh - 76px) !important;
}
</style>

<script>
  import menu from '@/config/menu-config'

  export default {
    data () {
      return {
        menu: menu
      }
    },
    methods: {
      handleOpen (key, keyPath) {
        console.log(key, keyPath)
      },
      handleClose (key, keyPath) {
        console.log(key, keyPath)
      }
    }
  }
</script>