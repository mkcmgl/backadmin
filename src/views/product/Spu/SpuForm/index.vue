<template>
  <div>
    <el-form :model="a" label-width="80">
      <el-form-item label="SPU名称">
        <el-input placeholder="请输入SPU名称"></el-input>
      </el-form-item>
      <el-form-item label="品牌">
        <el-select placeholder="请选择品牌" value="lavlue">
          <el-option :label="label" :value="va"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="描述">
        <el-input placeholder="请输入描述" type="textarea" rows="4"></el-input>
      </el-form-item>
      <el-form-item label="SPU图片">
        <el-upload
          action="https://jsonplaceholder.typicode.com/posts/"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
        >
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="" />
        </el-dialog>
      </el-form-item>
      <el-form-item label="销售属性">
        <el-select placeholder="还有三未选择">
          <el-option :label="label" :value="va"></el-option>
        </el-select>
        <el-button type="primary" icon="el-icon-plus">添加销售属性</el-button>
        <el-table style="100%" border>
          <el-table-column
            width="80"
            type="index"
            align="center"
            label="序号"
          ></el-table-column>
          <el-table-column
            prop="prop"
            label="属性值名称列表"
            align="center"
            width="width"
          ></el-table-column>
          <el-table-column
            align="center"
            prop="prop"
            width="width"
            label="操作"
          ></el-table-column>
        </el-table>
        <el-form-item>
          <el-button type="primary">保存</el-button>
          <el-button @click="cancel">取消</el-button>
        </el-form-item>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "SpuForm",

  data() {
    return {
      ialogImageUrl: "",
      dialogVisible: false,
      spu: {},
      tradeMarkList: [],
      spuImageList:[],
      saleAttrList:[],


    };
  },

  mounted() {},

  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    cancel() {
      this.$emit("changeScene", { scenne: 0, flag: "" });
      Object.assign(this._data, this.$options.data());
    },
    async initSpuData(id) {
      let result = await this.$API.spu.reqSpu(id);
      if (result.code == 200) {
        this.spu = result.data;
      }
      let tradeMarkResult = await this.$API.spu.reqTradeMarkList();
      if (tradeMarkResult.code == 200) {
        this.tradeMarkList = tradeMarkResult.data;
      }
      let reqSpuImageResult=await this.$API.spu.reqSpuImageList();
      if(reqSpuImageResult.code==200){
        this.spuImageList=reqSpuImageResult.data;
      } 
      let  saleResult=await this.$API.spu.reqBaseSaleAttrList()
      if(saleResult.code==200){
        this.saleAttrList=saleResult.data;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
</style>