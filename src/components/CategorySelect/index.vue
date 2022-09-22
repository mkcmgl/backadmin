<template>
  <div>
    <el-form :inline="true" class="demo-form-inline" :model="cForm">
      <!-- <el-form-item label="审批人">
              <el-input v-model="formInline.user" placeholder="审批人"></el-input>
            </el-form-item> -->
      <el-form-item label="一级分类">
        <el-select
          placeholder="请选择"
          v-model="cForm.category1Id"
          @change="handler1"
          :disabled="show"
        >
          <el-option
            :label="c1.name"
            :value="c1.id"
            v-for="(c1, index) in list1"
            :key="c1.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="二级分类">
        <el-select
          placeholder="请选择"
          v-model="cForm.category2Id"
          @change="handler2"
          :disabled="show"
        >
          <el-option
            :label="c2.name"
            :value="c2.id"
            v-for="(c2, index) in list2"
            :key="c2.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="三级分类">
        <el-select
          placeholder="请选择"
          v-model="cForm.category3Id"
          @change="handler3"
          :disabled="show"
        >
          <el-option
            :label="c3.name"
            :value="c3.id"
            v-for="(c3, index) in list3"
            :key="c3.id"
          ></el-option>
        </el-select>
      </el-form-item>
      <!-- <el-form-item>
              <el-button type="primary" @click="onSubmit">查询</el-button>
            </el-form-item> -->
    </el-form>
  </div>
</template>

<script>
export default {
  name: "CategorySelect",
  props:["show"],

  data() {
    return {
      list1: [],
      //二级分类的数据
      list2: [],
      //三级分类的数据
      list3: [],
      //收集相应的一级二级三级分类的id
      cForm: {
        category1Id: "",
        category2Id: "",
        category3Id: "",
      },
    };
  },

  mounted() {
    this.getCategory1List();
  },

  methods: {
    getCategory1List() {
      return new Promise((resolve, reject) => {
        this.$API.attr
          .reqCategory1List()
          .then((response) => {
            const { data } = response;
            console.log(data);
            this.list1 = data;
            resolve();
          })
          .catch((error) => {
            reject(error);
          });
      });
    },
    handler1() {
      //清除数据

      this.list2 = [];
      this.list3 = [];
      this.cForm.category2Id = "";
      this.cForm.category3Id = "";
      const { category1Id } = this.cForm;
      this.$emit("getCategoryId",{categoryId:category1Id,level:1});

      return new Promise((resolve, reject) => {
        this.$API.attr
          .reqCategory2List(category1Id)
          .then((response) => {
            const { data } = response;

            this.list2 = data;
            resolve();
          })
          .catch((error) => {
            reject(error);
          });
      });
    },
    handler2() {
      //清除数据
      this.list3 = [];
      this.cForm.category3Id = "";
      const { category2Id } = this.cForm;
      this.$emit("getCategoryId",{categoryId:category2Id,level:2});
      return new Promise((resolve, reject) => {
        this.$API.attr
          .reqCategory3List(category2Id)
          .then((response) => {
            const { data } = response;

            this.list3 = data;
            resolve();
          })
          .catch((error) => {
            reject(error);
          });
      });
    },
    handler3() {
      const { category3Id } = this.cForm;
        this.$emit("getCategoryId", { categoryId: category3Id, level: 3 });
    },
  },
};
</script>

<style lang="scss" scoped>
</style> 