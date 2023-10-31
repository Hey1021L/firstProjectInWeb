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
      <el-table-column type="index" label="序号" width="80" align="center" />
      <el-table-column
        v-for="(item, index) in columns"
        :key="index"
        :prop="item.prop"
        :label="item.label"
        :width="item.width"
        align="center"
      >
        <template v-slot="{ column, row }">
          <div v-if="column.property === 'opts'">
            <el-button type="primary" @click="amend(row.id)">修改</el-button>
            <el-button type="danger" @click="del(row.id)">删除</el-button>
          </div>
          <div v-else-if="column.property === 'energyState'">
            {{ row[column.property] === 1 ? '正常' :'异常' }}
          </div>
          <div v-else>{{ row[column.property] }}</div>
        </template>
      </el-table-column>
    </el-table>

    <!-- 弹出层 -->
    <el-dialog
      class="dialogform"
      :title="dialogTitle"
      :visible.sync="dialogFormVisible"
      width="30%"
    >
      <el-form :model="form" :rules="rules">
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item
              prop="detectionName"
              label="检测点名称"
              :label-width="formLabelWidth"
            >
              <el-input
                v-model="form.detectionName"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item
              prop="detectionAddress"
              label="检测点地址"
              :label-width="formLabelWidth"
            >
              <el-input
                v-model="form.detectionAddress"
                autocomplete="off"
              ></el-input> </el-form-item
          ></el-col>
        </el-row>
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item
              prop="power"
              label="实时功率"
              :label-width="formLabelWidth"
            >
              <el-input v-model="form.power" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item
              prop="voltage"
              label="实时电压"
              :label-width="formLabelWidth"
            >
              <el-input v-model="form.voltage" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item
              prop="energyState"
              label="状态"
              :label-width="formLabelWidth"
            >
              <el-select v-model="form.energyState" placeholder="请选择状态">
                <el-option label="正常" :value="1"></el-option>
                <el-option label="挂起" :value="0"></el-option>
              </el-select>
            </el-form-item>
          </el-col>
          <el-col :xs="24" :sm="24" :md="24" :lg="20" :xl="20">
            <el-form-item label="备注" :label-width="formLabelWidth">
              <el-input
                type="textarea"
                v-model="form.remark"
                autocomplete="off"
              ></el-input>
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
      //是否显示弹出层
      dialogFormVisible: false,
      //显示层标题
      dialogTitle: "",
      dialogAdd: false,
      //弹出层文字长度
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
          label: "检测点名称",
          prop: "detectionName",
          width: "100",
        },
        {
          label: "检测点地址",
          prop: "detectionAddress",
          width: "",
        },
        {
          label: "实时功率",
          prop: "power",
          width: "100",
        },
        {
          label: "实时电压",
          prop: "voltage",
          width: "100",
        },
        {
          label: "状态",
          prop: "energyState",
          width: "100",
        },
        {
          label: "上报时间",
          prop: "createTime",
          width: "150",
        },
        {
          label: "备注",
          prop: "remark",
          width: "",
        },
        {
          label: "操作",
          prop: "opts",
          width: "250",
        },
      ],
      form: {
        id: 0,
        detectionName: "",
        detectionAddress: "",
        power: "",
        voltage: "",
        energyState: "",
        createTime: "",
        remark: "",
      },
      formOld: {},
      //验证规则
      rules: {
        detectionName: [
          {
            required: true,
            message: "输入检测点名称",
            trigger: "blur",
          },
        ],
        detectionAddress: [
          {
            required: true,
            message: "输入检测点地址",
            trigger: "blur",
          },
        ],
        power: [
          {
            required: true,
            message: "输入实时功率",
            trigger: "blur",
          },
        ],
        voltage: [
          {
            required: true,
            message: "输入实时电压",
            trigger: "blur",
          },
        ],
        energyState: [
          {
            required: true,
            message: "请选择状态",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    //  搜索
    search() {
      console.log("搜索");
    },
    //  重置
    reset() {
      console.log(this.formatted());
      console.log("搜索");
    },
    //  新增
    add() {
      this.form = JSON.parse(JSON.stringify(this.formOld));
      this.dialogFormVisible = true;
      this.dialogTitle = "新增";

      const testJson = require("@/assets/energy.json");
      let list = testJson;
      this.tableData.push(
        ...(list = list.map((item) => {
          if (typeof item.power === "number") {
            item.power = item.power.toFixed(1) + "W";
            item.voltage = item.voltage.toFixed(1) + "V";
          }
          return item;
        }))
      );
      console.log(this.tableData);
    },
    //  修改
    amend(id) {
      this.dialogFormVisible = true;
      this.form = this.tableData.filter((item) => item.id === id)[0];
      this.dialogTitle = "修改";
    },
    //  删除
    del(id) {
      console.log(id);
      this.tableData = this.tableData.filter((item) => item.id !== id);
      console.log(this.tableData);
    },

    //格式化数据
    formatted() {
      const testJson = require("@/assets/energy.json");
      let list = JSON.parse(JSON.stringify(testJson));
      list = list.map((item) => {
        item.energyState = item.energyState == 1 ? "正常" : "异常";
        if (typeof item.power === "number") {
          item.power = item.power.toFixed(1) + "W";
          item.voltage = item.voltage.toFixed(1) + "V";
        }
        return item;
      });
      return list;
    },
  },
  mounted() {
    this.formOld = JSON.parse(JSON.stringify(this.form));
    const testJson = require("@/assets/energy.json");
    let list = JSON.parse(JSON.stringify(testJson));
    this.tableData = list.map((item) => {
      if (typeof item.power === "number") {
        item.power = item.power.toFixed(1) + "W";
        item.voltage = item.voltage.toFixed(1) + "V";
      }
      return item;
    });
    console.log(this.tableData);
    console.log(list);
  },
};
</script>

<style>
</style>
