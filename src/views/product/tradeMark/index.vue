<template>
  <div>
    <el-button
      type="primary"
      icon="el-icon-plus"
      style="margin: 10px 0px"
      @click="showDialog"
      >添加</el-button
    >

    <el-table :data="list" border style="width: 100%">
      <el-table-column
        type="index"
        label="序号"
        width="80px"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="tmName"
        label="品牌名称"
        width="width"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="logoUrl"
        label="品牌LOGO"
        width="width"
        align="center"
      >
        <template slot-scope="{ row, $index }">
          <img :src="row.logoUrl" alt="" style="width: 100px; height: 100px" />
        </template>
      </el-table-column>
      <el-table-column prop="prop" label="操作" width="width" align="center">
        <template slot-scope="{ row, $index }">
          <el-button
            type="warning"
            icon="el-icon-edit"
            size="mini"
            @click="updateTradeMark(row)"
            >修改</el-button
          >
          <el-button
            type="danger"
            icon="el-icon-delete"
            size="mini"
            @click="deleteTradeMark(row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <!-- 
      分页器 
      当前第几页、数据总条数、每一页展示条数、连续页码数
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"

      current-page:代表的是当前第几页
      total：代表分页器一共需要展示数据条数
      page-size：代表的是每一页需要展示多少条数据
      page-sizes：代表可以设置每一页展示多少条数据
      layout：可以实现分页器布局
      pager-count:按钮的数量  如果 9  连续页码是7

    -->
    <el-pagination
      style="margin-top: 20px; text-align: center"
      :total="total"
      :current-page="page"
      :page-size="limit"
      :page-count="7"
      :page-sizes="[3, 5, 10]"
      @current-change="getPageList"
      @size-change="handleSizeChange"
      layout="prev, pager, next, jumper,->,sizes,total"
    >
    </el-pagination>

    <el-dialog
      :title="tmForm.id ? '修改品牌' : '添加品牌'"
      :visible.sync="dialogFormVisible"
    >
      <el-form style="width: 80%" :model="tmForm" :rules="rules" ref="ruleForm">
        <el-form-item label="品牌名称" label-width="100px" prop="tmName">
          <el-input v-model="tmForm.tmName" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="品牌LOGO" label-width="100px" prop="logoUrl">
          <el-upload
            class="avatar-uploader"
            action="/dev-api/admin/product/fileUpload"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload"
          >
            <img v-if="tmForm.logoUrl" :src="tmForm.logoUrl" class="avatar" />
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>

            <div slot="tip" class="el-upload__tip">
              只能上传jpg/png文件，且不超过2Mb
            </div>
          </el-upload>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addOrUpdateTradeMark"
          >确 定</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>

<script>

export default {
  name: "tradeMark",

  data() {
    var validateTmName = (rule, value, callback) => {
      //自定义校验规则
      if (value.length < 2 || value.length > 10) {
        callback(new Error("品牌名称2-10位"));
      } else {
        callback();
      }
    };
    return {
      page: 1,
      limit: 3,
      total: 0,
      list: [],
      dialogFormVisible: false,
      tmForm: {
        tmName: "",
        logoUrl: "",
      },
      rules: {
        tmName: [
          { required: true, message: "请输入品牌名称", trigger: "blur" },
          //自定义校验规则
          { validator: validateTmName, trigger: "change" },
        ],
        //品牌的logo验证规则
        logoUrl: [{ required: true, message: "请选择品牌的图片" }],
      },
    };
  },

  mounted() {
    this.getPageList();
  },

  methods: {
    getPageList(pager = 1) {
      this.page = pager;
      const { page, limit } = this;
      return new Promise((resolve, reject) => {
        this.$API.trademark
          .reqTradeMarkList(page, limit)
          .then((response) => {
            const { data } = response;
            console.log(data);
            this.total = data.total;
            this.list = data.records;
            resolve();
          })
          .catch((error) => {
            reject(error);
          });
      });
    },
    //当分页器某一页需要展示数据条数发生变化的时候会触发
    handleSizeChange(limit) {
      //整理参数
      this.limit = limit;
      this.getPageList();
    },
    //点
    showDialog() {
      this.dialogFormVisible = true;
      this.tmForm = { tmName: "", logoUrl: "" };
    },
    updateTradeMark(row) {
      this.dialogFormVisible = true;
      this.tmForm = { ...row };
    },
    deleteTradeMark() {
      this.dialogFormVisible = true;
    },
    handleAvatarSuccess(res, file) {
      this.tmForm.logoUrl = URL.createObjectURL(file.raw);
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === "image/jpeg";
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error("上传头像图片只能是 JPG 格式!");
      }
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 2MB!");
      }
      return isJPG && isLt2M;
    },
    addOrUpdateTradeMark() {
      this.$refs.ruleForm.validate((success) => {
        if (success) {
          this.dialogFormVisible = false;
          return new Promise((resolve, reject) => {
            this.$API.trademark
              .reqAddOrUpdateTradeMark(this.tmForm)
              .then((response) => {
                this.$message({
                  type: this.tmForm.id ? "success" : "error",
                  message: this.tmForm.id ? "修改品牌成功" : "添加品牌成功",
                });
                this.getPageList(this.tmForm.id ? this.page : 1);
                resolve();
              })
              .catch((error) => {
                reject(error);
              });
          });
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    deleteTradeMark(row) {
      this.$confirm(`你确定删除${row.tmName}?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          // console.log(this.$API)
          // return new Promise((resolve, reject) => {
          //   console.log(this.$API)
          //   this.$API.trademark.reqDeleteTradeMark(row.id).then((response) => {

          //     this.$message({
          //       type: "success",
          //       message: "删除成功!",
          //     });
          //       //再次获取品牌列表数据
          //   this.getPageList(this.list.length > 1 ? this.page : this.page - 1);
          //   // resolve();
          //   }).catch((error)=>{
          //     // reject(error);
          //   });
          // });
          this.$API.trademark.reqDeleteTradeMark(row.id).then((res) => {
            if (res.code == 200) {
              this.$message({
                type: "success",
                message: "删除成功!",
              });
              this.getPageList(
                this.list.length > 1 ? this.page : this.page - 1
              );
            }
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
  },
};
</script>

<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}

.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}

.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>

<style lang="scss" scoped></style>
