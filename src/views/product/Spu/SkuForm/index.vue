<template>
    <div>
        <el-form ref="form" label-width="80px" >
          <el-form-item label="SPU名称">名称</el-form-item>
          <el-form-item label="SKU名称">
            <el-input  placeholder="请输入SKU名称"></el-input>
          </el-form-item>
          <el-form-item label="价格(元)"></el-form-item>
          <el-input  placeholder="请输入价格(元)"></el-input>
          <el-form-item label="重量(千克)">
            <el-input  placeholder="请输入重量(千克)"></el-input>
          </el-form-item>
          <el-form-item label="规格描述">
            <el-input type="textarea"  placeholder="请输入规格描述" rous="4"></el-input>
          </el-form-item>
          <el-form-item label="平台属性" >
            <el-form :inline="true" ref="form" label-width="80">
              <el-form-item label="屏幕属性">
                <el-select  placeholder="请选择">
                  <el-option label="label" value="value"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="无线通信">
                <el-select  placeholder="请选择">
                  <el-option label="label" value="value"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </el-form-item>
          <el-form-item label="销售属性">
            <el-form :inline="true" ref="form" label-width="80">
                <el-form-item label="颜色">
                  <el-select  placeholder="请选择">
                    <el-option label="label" value="value"></el-option>
                  </el-select>
                </el-form-item>

              </el-form>
          </el-form-item>
          <el-form-item label="图片列表">
            <el-table border style="width: 100%;" >
              <el-table-column type="selection"  prop="prop" width="80">

              </el-table-column>
              <el-table-column label="图片" prop="prop" width="width">

            </el-table-column>
            <el-table-column label="名称" prop="prop" width="width">

            </el-table-column>
            <el-table-column label="操作" prop="prop" width="width">

            </el-table-column>
            </el-table>
          </el-form-item>
          <el-form-item >
            <el-button type="primary">保存</el-button>
            <el-button >取消</el-button>
          </el-form-item>
        </el-form>
    </div>
</template>

<script>
export default {
    name: 'SkuForm',

    data() {
        return {
            
        };
    },

    mounted() {
        
    },

    methods: {
        
    },
};
</script>

<style lang="scss" scoped>

</style>