<template>
  <div>
    <el-form ref="form" label-width="80px">
      <el-form-item label="SPU名称">名称</el-form-item>
      <el-form-item label="SKU名称">
        <el-input placeholder="请输入SKU名称"></el-input>
      </el-form-item>
      <el-form-item label="价格(元)"></el-form-item>
      <el-input placeholder="请输入价格(元)"></el-input>
      <el-form-item label="重量(千克)">
        <el-input placeholder="请输入重量(千克)"></el-input>
      </el-form-item>
      <el-form-item label="规格描述">
        <el-input
          type="textarea"
          placeholder="请输入规格描述"
          rous="4"
        ></el-input>
      </el-form-item>
      <el-form-item label="平台属性">
        <el-form :inline="true" ref="form" label-width="80">
          <el-form-item label="屏幕属性">
            <el-select placeholder="请选择">
              <el-option label="label" value="value"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="无线通信">
            <el-select placeholder="请选择">
              <el-option label="label" value="value"></el-option>
            </el-select>
          </el-form-item>
        </el-form>
      </el-form-item>
      <el-form-item label="销售属性">
        <el-form :inline="true" ref="form" label-width="80">
          <el-form-item label="颜色">
            <el-select placeholder="请选择">
              <el-option label="label" value="value"></el-option>
            </el-select>
          </el-form-item>
        </el-form>
      </el-form-item>
      <el-form-item label="图片列表">
        <el-table border style="width: 100%">
          <el-table-column type="selection" prop="prop" width="80">
          </el-table-column>
          <el-table-column label="图片" prop="prop" width="width">
          </el-table-column>
          <el-table-column label="名称" prop="prop" width="width">
          </el-table-column>
          <el-table-column label="操作" prop="prop" width="width">
          </el-table-column>
        </el-table>
      </el-form-item>
      <el-form-item>
        <el-button type="primary">保存</el-button>
        <el-button>取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "SkuForm",

  data() {
    return {
      spuImageList: [],
      spuSaleAttrList: [],
      //存储平台属性的数据
      attrInfoList: [],
      //收集sku数据的字段
      skuInfo: {
        //第一类收集的数据：父组件给的数据
        category3Id: 0,
        spuId: 0,
        tmId: 0,
        //第二类：需要通过数据双向绑定v-model收集
        skuName: "",
        price: 0,
        weight: "",
        skuDesc: "",
        //第三类：需要自己书写代码
        //默认图片
        skuDefaultImg: "",
        //收集图片的字段
        skuImageList: [
          // {
          //   id: 0,
          //   imgName: "string",
          //   imgUrl: "string",
          //   isDefault: "string",
          //   skuId: 0,
          //   spuImgId: 0,
          // },
        ],
        //平台属性
        skuAttrValueList: [
          // {
          //   attrId: 0,
          //   valueId: 0,
          // },
        ],
        //销售属性
        skuSaleAttrValueList: [
          // {
          //   id: 0,
          //   saleAttrId: 0,
          //   saleAttrName: "string",
          //   saleAttrValueId: 0,
          //   saleAttrValueName: "string",
          //   skuId: 0,
          //   spuId: 0,
          // },
        ],
      },
      spu: {},
      //收集图片的数据字段:但是需要注意，收集的数据目前缺少isDefault字段，将来提交给服务器数据的时候，需要整理参数
      imageList: [],
    };
  },

  mounted() {},

  methods: {
    async getData(category1Id, category2Id, spu) {
      this.skuInfo.category3Id = spu.category3Id;
      this.skuInfo.spuId = spu.id;
      this.skuInfo.tmId = spu.tmId;
      this.spu = spu;
      let result = await this.$API.spu.reqSpuImageLIst(spu.id);
      if (result.code == 200) {
        this.spuImageList = result.data;
      }
      let saleList = await this.$API.spu.spuSaleAttrList(spu.id);
      if (saleList.code == 200) {
        this.spuSaleAttrList = saleList.data;
      }
      let infoList = await this.$API.spu.reqAttrInfoList(
        category1Id,
        category2Id,
        spu.category3Id
      );
      if (infoList.code == 200) {
        this.attrInfoList = infoList.data;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
</style>