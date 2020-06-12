<template>
  <div class="manage">
    <el-dialog :title="operateType === 'add' ? '新增用户信息' : '编辑用户信息'" :visible.sync="isShow">
      <common-form :formLabel="operateFormLabel" :form="operateForm"></common-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="isShow = false">取 消</el-button>
        <el-button type="primary" @click="confirm">确 定</el-button>
      </div>
    </el-dialog>
    <div class="manage-header">
      <el-button type="primary" size="medium" @click="addUser">+ 新增</el-button>
      <common-form inline :formLabel="formLabel" :form="searchForm">
        <el-button type="primary" icon="el-icon-search" size="medium" @click="getList(searchForm.keyword)"></el-button>
      </common-form>
    </div>
    <common-table :tableData="tableData" :tableLabel="tableLabel" :config="config" @changePage="getList" @edit="editUser" @del="delUser"></common-table>
  </div>
</template>

<script>
import CommonForm from "../../component/CommonForm"
import CommonTable from "../../component/CommonTable"
export default {
  components: {
    CommonForm,
    CommonTable
  },
  data() {
    return {
      isShow: false,
      operateType: "add",
      searchForm: {
        keyword: ""
      },
      formLabel: [
        {
          model: "keyword",
          label: ""
        }
      ],
      operateForm: {
        name: "",
        addr: "",
        age: "",
        birth: "",
        sex: ""
      },
      operateFormLabel: [
        {
          model: "name",
          label: "姓名"
        },
        {
          model: "age",
          label: "年龄"
        },
        {
          model: "sex",
          label: "性别",
          type: "select",
          opts: [
            {
              label: "男",
              value: 1
            },
            {
              label: "女",
              value: 0
            }
          ]
        },
        {
          model: "birth",
          label: "出生日期",
          type: "date"
        },
        {
          model: "addr",
          label: "地址"
        }
      ],
      //分页配置
      config: {
        page: 1,
        total: 30,
        loading: false
      },
      //表格内容
      tableData: [],
      //表格列名
      tableLabel: [
        {
          prop: "name",
          label: "姓名"
        },
        {
          prop: "age",
          label: "年龄"
        },
        {
          prop: "sexLabel",
          label: "性别"
        },
        {
          prop: "birth",
          label: "出生日期"
        },
        {
          prop: "addr",
          label: "地址"
        }
      ]
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList(name = "") {
      // this.config.page = page
      // console.log(name)
      this.config.loading = true
      // 搜索时，页码需要设置为1，才能正确返回数据，因为数据是从第一页开始返回的
      name ? (this.config.page = 1) : ""
      this.$http
        .get("/api/user/getUser", {
          params: {
            page: this.config.page,
            name
          }
        })
        .then(res => {
          this.tableData = res.data.list.map(item => {
            item.sexLabel = item.sex === 0 ? "女" : "男"
            return item
          })
          this.config.total = res.data.count
          this.config.loading = false
        })
    },
    addUser() {
      this.operateType = "add"
      this.isShow = true
      this.operateForm = {}
    },
    editUser(row) {
      this.operateType = "edit"
      this.isShow = true
      this.operateForm = row
    },
    delUser(row) {
      this.$confirm("此操作将永久删除该用户信息, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          let id = row.id
          this.$http
            .get("/api/user/del", {
              params: {
                id
              }
            })
            .then(res => {
              console.log(res.data)
              this.$message({
                type: "success",
                message: "删除成功!"
              })
              this.getList()
            })
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          })
        })
    },
    confirm() {
      if (this.operateType === "edit") {
        this.$http.post("/api/user/edit", this.operateForm).then(res => {
          this.isShow = false
          this.getList()
        })
      } else {
        this.$http.post("/api/user/add", this.operateForm).then(res => {
          console.log(res.data)
          this.isShow = false
          this.getList()
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/scss/common";
</style>
