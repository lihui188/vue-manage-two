<template>
  <el-container style="height:100%;">
    <el-aside width="auto">
      <common-aside></common-aside>
    </el-aside>
    <el-container>
      <el-header>
        <common-header></common-header>
      </el-header>
      <common-tab></common-tab>
      <el-main>
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
import CommonHeader from "../component/CommonHeader"
import CommonTab from "../component/CommonTab"
import CommonAside from "../component/CommonAside"
export default {
  data() {
    return {}
  },
  components: {
    CommonHeader,
    CommonAside,
    CommonTab
  }
}
</script>
<style lang="scss" scoped>
.el-header {
  background-color: #000;
}
// .el-main {
//   padding-top: 0;
// }
</style>
