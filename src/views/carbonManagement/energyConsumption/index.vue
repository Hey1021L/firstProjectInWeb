<template>
  <div>
    <el-form v-model="queryForm" class="header">
      <el-input
        placeholder="设备名称"
        label="设备名称"
        v-model="queryForm.name"
      ></el-input>
      <el-input
        placeholder="日期"
        label="日期"
        v-model="queryForm.data"
      ></el-input>
      <el-button type="primary" @click="query">搜索</el-button>
      <el-button type="primary" @click="reset">重置</el-button>
    </el-form>
    <el-table class="table-item" :data="dataForm" border stripe>
      <el-table-column
        type="index"
        label="序号"
        width="50"
        :align="center"
      ></el-table-column>
      <el-table-column
        v-for="(item, index) in columns"
        :key="index"
        :label="item.label"
        :prop="item.prop"
        :width="item.width"
        :align="center"
      ></el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      center: "center",
      queryForm: {
        name: "",
        date: "",
      },
      dataForm: [],
      columns: [
        {
          label: "设备名称",
          prop: "devicename",
          width: "250",
        },
        {
          label: "已产数量",
          prop: "output",
          width: "200",
        },
        {
          label: "单位耗电量",
          prop: "power",
          width: "250",
        },
        {
          label: "总耗电量",
          prop: "totalPower",
          width: "250",
        },
        {
          label: "单位碳排放",
          prop: "emission",
          width: "250",
        },

        {
          label: "总碳排放",
          prop: "totalEmission",
          width: "",
        },
      ],
    };
  },
  methods: {
    //搜索
    query() {},
    //重置
    reset() {},

    //添加列表
    setList() {
      const testJson = require("@/assets/energycollect.json");
      this.dataForm = testJson;
      console.log(testJson);
      console.log(this.dataForm);
    },
  },
  mounted() {
    this.setList();
  },
};
</script>

<style>
</style>