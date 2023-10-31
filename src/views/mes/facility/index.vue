<template>
  <div>
    <!-- 头部新增搜索重置功能 -->
    <el-form v-model="queryForm" class="header">
      <el-input
        placeholder="编码"
        label="编码"
        v-model="queryForm.encoding"
      ></el-input>
      <el-input
        placeholder="型号"
        label="型号"
        v-model="queryForm.model"
      ></el-input>
      <el-date-picker
        v-model="queryForm.batchDate"
        type="date"
        placeholder="选择日期"
      >
      </el-date-picker>
      <el-input
        placeholder="生产编号"
        label="生产编号"
        v-model="queryForm.batchNumber"
      ></el-input>
      <el-input
        placeholder="制造商"
        label="制造商"
        v-model="queryForm.manufacturer"
      ></el-input>
      <el-select v-model="queryForm.state" placeholder="设备状态">
        <el-option label="空闲" value="1"></el-option>
        <el-option label="使用中" value="2"></el-option>
        <el-option label="故障" value="3"></el-option>
      </el-select>
      <el-button type="primary" @click="query">搜索</el-button>
      <el-button type="primary" @click="reset">重置</el-button>
      <el-button type="primary" @click="add">新建</el-button>
    </el-form>

    <el-table :data="dataForm" border stripe>
      <el-table-column
        v-for="(item, index) in columns"
        :key="index"
        :label="item.label"
        :prop="item.prop"
        :width="item.width"
        :align="center"
      >
        <template v-slot="{ column, row }">
          <div v-if="column.property === 'opts'">
            <el-button type="primary">修改</el-button>
            <el-button type="danger">删除</el-button>
          </div>
          <div v-else-if="column.property === 'state'">
            <el-select v-model="row[column.property]" disabled="true">
              <el-option label="空闲" value="1"></el-option>
              <el-option label="使用中" value="2"></el-option>
              <el-option label="故障" value="3"></el-option>
            </el-select>
          </div>
          <div v-else>
            {{ row[column.property] }}
          </div>
        </template>
      </el-table-column>
    </el-table>

    <!-- 新增弹出层 -->
    <el-dialog
      title="添加设备信息"
      width="30%"
      :visible.sync="dialogFormVisible"
    >
      <el-form :model="addForm">
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item
              prop="facilityName"
              label="名称"
              :label-width="formLabelWidth"
            >
              <el-input
                v-model="addForm.facilityName"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item
              prop="encoding"
              label="编码"
              :label-width="formLabelWidth"
            >
              <el-input
                v-model="addForm.encoding"
                autocomplete="off"
              ></el-input> </el-form-item
          ></el-col>
        </el-row>
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item label="型号" :label-width="formLabelWidth">
              <el-input v-model="addForm.model" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="生产日期" :label-width="formLabelWidth">
              <el-date-picker
                v-model="addForm.batchDate"
                type="date"
                placeholder="选择日期"
              >
              </el-date-picker>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item label="生产批号" :label-width="formLabelWidth">
              <el-input
                v-model="addForm.batchNumber"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item label="制造商" :label-width="formLabelWidth">
              <el-input
                v-model="addForm.manufacturer"
                autocomplete="off"
              ></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item label="设备状态" :label-width="formLabelWidth">
              <el-select v-model="addForm.state" placeholder="请选择设备状态">
                <el-option label="空闲" value="1"></el-option>
                <el-option label="使用中" value="2"></el-option>
                <el-option label="故障" value="3"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>

      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false"
          >确 定</el-button
        >
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //搜索框
      queryForm: {
        encoding: "",
        model: "",
        batchDate: "",
        batchNumber: "",
        manufacturer: "",
        state: "",
      },
      //弹出层
      dialogFormVisible: false,

      center: "center",

      //列表数据
      dataForm: [],

      //显示列表
      columns: [
        {
          label: "编号",
          prop: "id",
          width: "100",
        },
        {
          label: "名称",
          prop: "facilityName",
          width: "100",
        },
        {
          label: "编码",
          prop: "encoding",
          // width: "200",
        },
        {
          label: "型号",
          prop: "model",
          width: "100",
        },
        {
          label: "生产日期",
          prop: "batchDate",
          width: "200",
        },
        {
          label: "生产批号",
          prop: "batchNumber",
          // width: "200",
        },
        {
          label: "制造商",
          prop: "manufacturer",
          // width: "200",
        },
        {
          label: "设备状态",
          prop: "state",
          width: "120",
        },
        {
          label: "操作",
          prop: "opts",
          width: "250",
        },
      ],

      formLabelWidth: "100px",
      //新建表单
      addForm: {
        facilityName: "",
        encoding: "",
        model: "",
        batchDate: "",
        batchNumber: "",
        manufacturer: "",
        state: "",
      },
    };
  },
  methods: {
    //搜索
    query() {},
    //重置
    reset() {},
    //新增
    add() {
      this.dialogFormVisible = true;
    },
  },
  mounted() {
    const testJson = require("@/assets/facility.json");
    let list = testJson;
    this.dataForm = list.map((item) => {
      item.state = item.state.toString();
      return item;
    });
  },
};
</script>

<style>
</style>