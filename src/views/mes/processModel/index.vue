<template>
  <div class="page">
    <div class="block">
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
      <el-table-column prop="date" label="日期" width="180" />
      <el-table-column prop="name" label="姓名" width="180" />
      <el-table-column prop="address" label="地址" />
      <el-table-column prop="button" label="操作">
        <div>
          <el-button type="primary" @click="amend">修改</el-button>
          <el-button type="danger" @click="del">删除</el-button>
        </div>
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
export default {
  data() {
    return {
      //是否显示弹出层
      dialogFormVisible: false,
      //弹出层输入框长度
      formLabelWidth: "100px",
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄",
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
        },
      ],
      tableInfo: {
        detectionName: "",
        detectionAddress: "",
        energyState: "",
      },
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
    reset() {},
    //  新增
    add() {},
    //  修改
    amend() {
      this.dialogFormVisible = true;
    },
    //  删除
    del() {},
  },
};
</script>

<style>
</style>
