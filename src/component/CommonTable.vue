<template>
  <div class="common-table">
    <el-table :data="tableData" height="90%" stripe v-loading="config.loading">
      <!-- 序号列 -->
      <el-table-column label="序号" width="85">
        <template slot-scope="scope">
          <!-- <i class="el-icon-time"></i> -->
          <!-- 序号的编号，分页，每一页有20条数据，第2页，序号就加上20，以此类推 -->
          <span style="margin-left:10px">{{ (config.page - 1) * 20 + scope.$index + 1 }}</span>
        </template>
      </el-table-column>
      <!-- 自定义列名，进行v-for渲染 -->
      <el-table-column show-overflow-tooltip stripe :label="item.label" v-for="item in tableLabel" :key="item.prop" :width="item.prop !== 'addr' ? '120' : ''">
        <template slot-scope="scope">
          <!-- <i class="el-icon-time"></i> -->
          <span style="margin-left:10px">{{ scope.row[item.prop] }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="150">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination class="pager" layout="prev,pager,next" :total="config.total" :current-page.sync="config.page" @current-change="changePage"></el-pagination>
  </div>
</template>
<script>
export default {
  props: {
    tableData: Array,
    tableLabel: Array,
    config: Object
  },
  methods: {
    handleEdit(row) {
      this.$emit("edit", row)
    },
    handleDelete(row) {
      this.$emit("del", row)
    },
    changePage(page) {
      // console.log(page)
      this.$emit("changePage", page)
    }
  }
}
</script>
<style lang="scss" scoped>
.common-table {
  height: calc(100% - 62px);
  background-color: #fff;
  position: relative;
  .pager {
    position: absolute;
    right: 20px;
    bottom: 0;
  }
}
</style>
