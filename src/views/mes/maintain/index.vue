<template>
  <div>
    <div class="header">
      <el-input
        v-model="tableData.detectionName"
        placeholder="检测点名称"
        style="width: 150px"
      />
      <el-input
        placeholder="检测点地址"
        v-model="tableData.detectionAddress"
        style="width: 150px"
      />
      <el-input
        placeholder="状态"
        v-model="tableData.energyState"
        style="width: 100px"
      />
      <el-button type="primary" @click="search">搜索</el-button>
      <el-button type="primary" @click="reset">重置</el-button>
      <el-button type="primary" @click="add">新增</el-button>
    </div>
    <el-table :data="tableData" border stripe style="width: 100%">
      <el-table-column type="index" label="序号" width="100"  align="center"/>
      <el-table-column v-for="(item,index) in columns" :key="index" :prop="item.prop" :label="item.label" :width="item.width" align="center">
      <template v-slot="{column,row}">
        <div v-if="column.property === 'opts'">
          <el-button type="primary" @click="amend">修改</el-button>
          <el-button type="danger" @click="del">删除</el-button>
        </div> <div v-else>{{ row[column.property] }}</div>
      </template>
      </el-table-column>
      </el-table-column>
    </el-table>

    <!-- 弹出层 -->
    <el-dialog title="收货地址" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="活动名称" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="活动区域" :label-width="formLabelWidth">
          <el-select v-model="form.region" placeholder="请选择活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
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
const testJson = require("@/assets/preserve.json");
export default {
  data() {
    return {
      //是否显示弹出层
      dialogFormVisible: false,
      //弹出层输入框长度
      formLabelWidth: "100px",
      tableData: [],
      tableInfo: {
        detectionName: "",
        detectionAddress: "",
        energyState: "",
      },
      columns: [
        {
          label: "编号",
          prop: "id",
          width: "100",
        },
        {
          label: "项目名",
          prop: "itemName",
          width: "200",
        },
        {
          label: "负责人",
          prop: "principal",
          width: "100",
        },
        {
          label: "开始日期",
          prop: "startDate",
          width: "150",
        },
        {
          label: "结束日期",
          prop: "endDate",
          width: "150",
        },
        {
          label: "项目状态",
          prop: "itemStatus",
          width: "100",
        },
        {
          label: "操作",
          prop: "opts",
        },
      ],
      form: {
        name: "",
        region: "shanghai",
      },
    };
  },
  methods: {
    //  搜索
    search() {},
    //  重置
    reset() {
      this.tableData = testJson;
    },
    //  新增
    add() {
      let list = testJson;
      this.tableData.push(...list);
    },
    //  修改
    amend() {
      this.dialogFormVisible = true;
    },
    //  删除
    del() {},
  },
  mounted() {
    let list = testJson;
    this.tableData.push(...list);
  },
};
</script>

<style>
</style>
