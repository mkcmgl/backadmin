<template>
  <div>
    <el-card>
      <CategorySelect
        @getCategoryId="getCategoryId"
        :show="scene != 0"
      ></CategorySelect>
    </el-card>
    <el-card>
      <!-- <div v-show="scene == 0"> -->
        <div v-show="scene==0">
        <el-button
          type="primary"
          @click="addSpu"
          icon="el-icon-plus"
          :disabled="!category3Id"
          >添加SPU</el-button
        >
        <el-table style="width: 100%" border :data="records">
            <el-table-column type="index" label="序号" width="80" align="center">
            </el-table-column>
            <el-table-column prop="spuName" label="SPU名称" width="width">
            </el-table-column>
            <el-table-column prop="description" label="SPU描述" width="width">
            </el-table-column>
            <el-table-column prop="prop" label="操作" width="width">
              <template slot-scope="{ row, $index }">
                <!-- 这里按钮将来用hintButton替换 -->
                <hint-button
                  type="success"
                  icon="el-icon-plus"
                  size="mini"
                  title="添加sku"
                  @click="addSku(row)"
                ></hint-button>
                <hint-button
                  type="warning"
                  icon="el-icon-edit"
                  size="mini"
                  title="修改spu"
                  @click="updateSpu(row)"
                ></hint-button>
                <hint-button
                  type="info"
                  icon="el-icon-info"
                  size="mini"
                  title="查看当前spu全部sku列表"
                  @click="handler(row)"
                ></hint-button>
                <el-popconfirm
                  title="这是一段内容确定删除吗？"
                  @onConfirm="deleteSpu(row)"
                >
                  <hint-button
                    type="danger"
                    icon="el-icon-delete"
                    size="mini"
                    title="删除spu"
                    slot="reference"
                  ></hint-button>
                </el-popconfirm>
              </template>
            </el-table-column>
          </el-table>
        <el-pagination
        style="margin-top: 20px; text-align: center"
        :current-page="page"
        :page-sizes="[10,20,50]"
        :page-size="limit"
        @current-change="getSpuList"
        @size-change="handleSizeChange"
        :total="total"
        layout="prev, pager, next, jumper,->, sizes,total"

      >
      </el-pagination>

      </div>
      <SpuForm @changeScene="changeScene" ref="spu" v-show="scene==1"></SpuForm>
      <SkuForm  v-show="scene==2"></SkuForm>
    </el-card>
  </div>
</template>

<script>
    import SkuForm from './SkuForm';
    import SpuForm from './SpuForm'

export default {
  name: "Spu",
    components:{ SpuForm, SkuForm },
  data() {
    return {
      category1Id: "",
      category2Id: "",
      category3Id: "",
      page: 1, //分页器当前第几页
      limit: 3, //每一页需要展示多少条数据
      records: [], //spu列表的数据
      total: 0, //分页器一共需要展示数据的条数
      scene: 0, //0代表展示SPU列表数据   1 添加SPU|修改SPU   2 添加SKU
      //控制对话框的显示与隐藏
      dialogTableVisible: false,
      spu: {},
      skuList: [], //存储的是SKU列表的数据
      loading: true,
    };
  },

  mounted() {},

  methods: {
    getCategoryId({categoryId,level}){
        if(level==1){
            this.category1Id=categoryId;
            this.category2Id='';
            this.category3Id='';
        }
        if(level==2){
            this.category2Id=categoryId;
            this.category3Id='';
        }
        if(level==3){
            this.category3Id=categoryId;
            this.getSpuList();
        }
    },
    async getSpuList(pages = 1) {
      this.page = pages;
      const { page, limit, category3Id } = this;
      //携带三个参数:page 第几页  limit 每一页需要展示多少条数据  三级分类id
      let result = await this.$API.spu.reqSpuList(page, limit, category3Id);
      if (result.code == 200) {
        this.total = result.data.total;
        this.records = result.data.records;
      }
    },

    handleSizeChange(limit){
        this.limit=limit;
        this.getSpuList();
    },
    addSpu(){
        this.scene=1;

    },
    updateSpu(row){
        this.scene=1;
        this.$refs.spu.initSpuData(row.id);

    },
    changeScene(){

    },
    deleteSpu(){},
    addSku(row){
        this.scene=2;
    },
    changeScenes(scene){
        this.scene=scene;
    },
    handler(){},
    close(){},
  },
};
</script>

<style  scoped>
</style>