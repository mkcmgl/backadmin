<template>
  <div>
    <el-card>
      <CategorySelect @getCategoryId="getCategoryId"></CategorySelect>
    </el-card>
    <el-card>
      <div v-show="isShowTanle">
        <el-button
          type="primary"
          icon="el-incon-plus"
          :disabled="!category3Id"
          @click="addAttr"
          >添加属性</el-button
        >

        <el-table :data="attrList" style="width: 100%" border>
          <el-table-column
            label="序号"
            type="index"
            align="center"
            with="80"
          ></el-table-column>
          <el-table-column
            label="属性名称"
            prop="attrName"
            align="centet"
            with="150"
          ></el-table-column>
          <el-table-column
            label="属性值列表"
            prop="prop"
            align="centet"
            with="with"
          >
            <template slot-scope="{ row, $index }">
              <el-tag
                type="success"
                v-for="(attrValue, index) in row.attrValueList"
                :key="attrValue.id"
                style="margin: 0px 20px"
                >{{ attrValue.valueName }}</el-tag
              >
            </template>
          </el-table-column>
          <el-table-column label="操作" prop="prop" align="centet" wit="150">
            <template slot-scope="{ row, $index }">
              <el-button
                type="primary"
                icon="el-icon-edit"
                size="mini"
                @click="updataAttr(row)"
              ></el-button>
              <el-button
                type="danger"
                icon="el-icon-delete"
                size="mini"
              ></el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div v-show="!isShowTanle">
        <el-form :inline="true" ref="form" label-width="80px" :model="attrInfo">
          <el-form-item label="属性名">
            <el-input
              v-model="attrInfo.attrName"
              placeholder="请输入属性名"
            ></el-input>
          </el-form-item>
        </el-form>
        <el-button
          type="primary"
          icon="el-icon-plus"
          @click="addAttrrValue"
          :disabled="!attrInfo.attrName"
          >添加属性值</el-button
        >
        <el-button @click="isShowTanle = true">取消</el-button>
        <el-table   style="width: 100%; margin: 20px 0px" border  :data="attrInfo.attrValueList">
          <el-table-column
            align="center"
            type="index"
            label="序号"
            width="80px"
          ></el-table-column>
          <el-table-column
            align="center"
            prop="prop"
            label="属性值名称"
            width="width"
          >
            <template slot-scope="{ row, $index }">
              <el-input
                v-model="row.valueName"
                placeholder="请输入属性值名称"
                v-if="flag"
                @blur="flag=false"
                @keyup.native.enter="flag=false"
              ></el-input>
              <span v-else @click="flag=true" style="display: block;">{{row.valueName}}</span>
            </template>
          </el-table-column>
          <el-table-column
            align="center"
            prop="prop"
            label="操作"
            width="width"
          >
            <template slot-scope="{ row, $index }">
              <el-button
                type="danger"
                icon="el-icon-delete"
                size="mini"
              ></el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-button type="primary">保存</el-button>
        <el-button @click="isShowTanle = true">取消</el-button>
      </div>
    </el-card>
  </div>
</template>

<script>
    import cloneDeep from 'lodash/cloneDeep';
export default {
  name: "Attr",
  data() {
    return {
      category1Id: "",
      category2Id: "",
      category3Id: "",
      attrList: [],
      isShowTanle: true,
      
      attrInfo: {
        attrName: "", //属性名
        attrValueList: [
          //属性值，因为属性值可以有多个因此用数组，每一个属性值都是一个对象需要attrId，valueName
        ],
        categoryId: 0, //三级分类的id
        categoryLevel: 3, //因为服务器也需要区分几级id
      },
    };
  },
  mounted() {},
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

        //发请求获取平台的属性数据
        this.getAttrList();
      }
    },
    getAttrList() {
      const { category1Id, category2Id, category3Id } = this;
      return new Promise((resolve, reject) => {
        this.$API.attr
          .reqAttrList(category1Id, category2Id, category3Id)
          .then((response) => {
            const { data } = response;

            this.attrList = data;
            resolve();
          })
          .catch((error) => {
            reject(error);
          });
      });
    },
    addAttrrValue() {
      
      this.attrInfo.attrValueList.push({
        attrId: this.attrInfo.id,
        ValueName: "",
        flag:true,
      });
    },
    addAttr() {
        this.isShowTanle=false;
        this.attrInfo={
        attrName: "", //属性名
        attrValueList: [
          //属性值，因为属性值可以有多个因此用数组，每一个属性值都是一个对象需要attrId，valueName
        ],
        categoryId: this.category3Id, //三级分类的id
        categoryLevel: 3, //因为服务器也需要区分几级id
      }
    },
    updataAttr(row){
        this.isShowTanle=false;
        this.attrInfo=cloneDeep(row);
    },
    addOrUpdateAttr() {},
  },
  //https://blog.csdn.net/qq_45659769/article/details/125970317
};
</script>

<style lang="scss" scoped>
</style>