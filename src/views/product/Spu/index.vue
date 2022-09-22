<template>
  <div>
    <el-card>
      <CategorySelect
        @getCategoryId="getCategoryId"
        :show="scene != 0"
      ></CategorySelect>
    </el-card>
    <el-card>
      <div v-show="scene == 0">
        <el-button
          type="primary"
          @click="addSpu"
          icon="el-icon-plus"
          :disabled="!category3Id"
          >添加SPU</el-button
        >
        <el-table :data="records" border style="100%">
          <el-table-column
            label="序号"
            type="index"
            align="center"
            width="80"
          ></el-table-column>
          <el-table-column
            label="SPU名称"
            prop="spuName"
            align="center"
            width="width"
          ></el-table-column>
          <el-table-column
            label="SPU描述"
            prop="description"
            align="center"
            width="width"
          ></el-table-column>
          <el-table-column
            label="操作"
            align="center"
            width="width"
            prop="prop"
          ></el-table-column>
          <template slot-scope="{ row, $index }">
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
              title="修改sku"
              @click="addSku(row)"
            ></hint-button>
            <hint-button
              type="info"
              icon="el-icon-info"
              size="mini"
              title="查看当前spu全部sku列表"
              @click="addSku(row)"
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
                @click="addSku(row)"
              ></hint-button>
            </el-popconfirm>
          </template>
        </el-table>
        <el-pagination
        style="text-align: center"
        :current-page="page"
        :page-sizes="[3, 5, 10]"
        :page-size="limit"
        layout="prev, pager, next, jumper,->, sizes,total"
        @current-change="getSpuList"
        @size-change="handleSizeChange"
        :total="total"
      >
      </el-pagination>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "Spu",

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
    getSpuList(pages = 1){
        this.page=pages;
        const {page,limit,category3Id}=this;
        this.$API.spu.reqSpuList(page,limit,category3Id).then((res)=>{
            console.log(res);
            this.total=res.total;
        }).catch((error)=> {
            this.$message(error);
        }) 
    },
    handleSizeChange(limit){
        this.limit=limit;
        this.getSpuList();
    },
    addSpu(){

    },
    updateSpu(){

    },
    changeScene(){

    },
    deleteSpu(){},
    addSku(){},
    changeScenes(){},
    handler(){},
    close(){},
  },
};
</script>

<style lang="scss" scoped>
</style>