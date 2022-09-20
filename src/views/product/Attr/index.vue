<template>
    <div>
        <el-card>
            <CategorySelect @getCategoryId="getCategoryId"></CategorySelect>
        </el-card>
        <el-card>
            <el-button type="primary" icon="el-incon-plus">添加属性</el-button>

            <el-table  :data="attrList" style="width:100%"  border>
              <el-table-column label="序号" type="index" align="center" with="80"></el-table-column>
              <el-table-column label="属性名称" prop="attrName" align="centet" with="150"></el-table-column>
              <el-table-column label="属性值列表" prop="prop"  align="centet"  with="with">
                <template slot-scope="{row,$index}">
                    <el-tag type="success" v-for="(attrValue,index) in row.attrValueList" :key="attrValue.id"  style="margin:0px 20px">{{ attrValue.valueName }}</el-tag>
                </template>

              </el-table-column>
              <el-table-column label="操作" prop="prop"  align="centet"  wit="150">
                <template slot-scope="{row,$index}">
                    <el-button type="warning" icon="el-icon-edit" size="mini"></el-button>
                    <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
                        
                    
                </template>
              </el-table-column>
            </el-table>
        </el-card>

    </div>
</template>

<script>

export default {
    name: "Attr",
    data() {
        return {
            category1Id:'',
            category2Id:'',
            category3Id:'',
            attrList:[],
        };
    },
    mounted() {
    },
    methods: {
        getCategoryId({ categoryId, level }) {
            if (level == 1) {
                this.category1Id = categoryId;
                this.category2Id = "";
                this.category3Id = "";
            } else if (level == 2) {
                this.category2Id = categoryId;
                this.category3Id = "";
            } else {
                //代表三级分类的id有了
                this.category3Id = categoryId;
                console.log('请求')
                //发请求获取平台的属性数据
                this.getAttrList();
            }
        },
        getAttrList(){
            const {category1Id,category2Id,category3Id}=this;
            return new Promise((resolve, reject) => {
                this.$API.attr.reqAttrList(category1Id,category2Id,category3Id).then((response)=>{
                    const {data}=response;
                    console.log(data)
                    this.attrList=data;
                    resolve();
                }).catch((error)=>{reject(error)})
            })
        }
    }
//https://blog.csdn.net/qq_45659769/article/details/125970317
 
};
</script>

<style lang="scss" scoped>

</style>