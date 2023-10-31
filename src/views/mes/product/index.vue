<template>
  <div>
    <div class="header">
      <el-input placeholder="模型名称" v-model="queryName" />
      <el-button type="primary" @click="query">搜索</el-button>
      <el-button type="primary" @click="reset">重置</el-button>
      <el-button type="primary" @click="add">新建</el-button>
    </div>

    <el-table :data="dataForm" border stripe>
      <el-table-column
        v-for="item in columns"
        :key="item.id"
        :label="item.label"
        :prop="item.prop"
        :width="item.width"
        :align="center"
      >
        <template v-slot="{ column, row }">
          <div v-if="column.property === 'opts'">
            <el-button type="primary" @click="revamp(row.id)">修改</el-button>
            <el-button type="danger">删除</el-button>
          </div>
          <div>{{ row[column.property] }}</div>
        </template>
      </el-table-column>
    </el-table>

    <div class="footer_pagination">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="[10, 15, 20]"
        :page-size="10"
        layout="total, sizes, prev, pager, next, jumper"
        :total="rows"
      >
      </el-pagination>
    </div>

    <!-- 新增弹出层 -->
    <el-dialog
      title="添加产品模型"
      :visible.sync="dialogFormVisible"
      width="30%"
    >
      <el-form :model="form">
        <el-row>
          <el-col :xl="20">
            <el-form-item label="产品" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="产品型号" :label-width="formLabelWidth">
              <el-input
                v-model="form.name"
                disabled
                autocomplete="off"
              ></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col>
            <el-form-item label="产品规格" :label-width="formLabelWidth">
              <el-input
                v-model="form.name"
                disabled
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item label="模型名称" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col>
            <el-form-item label="备注" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="添加物料" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
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

    <!-- 修改弹出层 -->
    <el-dialog title="修改产品模型" :visible.sync="dialogVisible" width="50%">
      <el-form :model="form">
        <el-row :gutter="20">
          <el-col :md="12" :lg="11" :xl="9">
            <el-form-item label="产品" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="12" :lg="11" :xl="9">
            <el-form-item label="产品型号" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="12" :lg="11" :xl="9">
            <el-form-item label="产品规格" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="12" :lg="11" :xl="9">
            <el-form-item label="模型名称" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="12" :lg="11" :xl="9">
            <el-form-item label="备注" :label-width="formLabelWidth">
              <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="12" :lg="11" :xl="9" style="text-align: center">
            <el-form-item label="选择物料" :label-width="formLabelWidth">
              <el-button type="primary">选择</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <hr />
      <!-- <div class="dotted-line"></div> -->
      <el-table>
        <el-table-column type="index" label="序号"></el-table-column>
        <el-table-column
          v-for="(item, index) in columns1"
          :key="index"
          :label="item.label"
          :prop="item.prop"
        >
          <template v-slot="{ column, row }">
            <div v-if="column.property === 'material'">
              <el-button type=" primary">选择</el-button>
            </div>
            <div v-else-if="column.property === 'opts'">
              <el-button type=" danger">删除</el-button>
            </div>
            <div>
              {{ row[column.property] }}
            </div>
          </template>
        </el-table-column>
      </el-table>
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
const testJson = require("@/assets/dtb_product_modeling.json");
export default {
  data() {
    return {
      center: "center",

      queryName: "",

      //列表数据
      dataForm: [],

      //测试 模拟后端数据库 做分页
      dataTotal: [],

      //产品模型显示列
      columns: [
        {
          label: "编号",
          prop: "id",
          width: "",
        },
        {
          label: "模型名称",
          prop: "modelName",
          width: "",
        },
        {
          label: "产品",
          prop: "product",
          width: "",
        },
        {
          label: "产品型号",
          prop: "productModeling",
          width: "",
        },
        {
          label: "产品规格",
          prop: "productStandard",
          width: "",
        },
        {
          label: "产品单位",
          prop: "productUnit",
          width: "",
        },
        {
          label: "操作",
          prop: "opts",
          width: "250",
        },
      ],

      //建模明细显示列
      columns1: [
        {
          label: "物料",
          prop: "material",
        },
        {
          label: "型号",
          prop: "model",
        },
        {
          label: "规格",
          prop: "specification",
        },
        {
          label: "单位",
          prop: "unit",
        },
        {
          label: "备注",
          prop: "remark",
        },
        {
          label: "操作",
          prop: "opts",
        },
      ],

      //物料信息显示列
      columns2: [],

      //数据条数
      rows: 0,

      //
      page: {
        pageNo: 1,
        pageSize: 10,
      },

      //弹出层
      dialogFormVisible: false,

      //修改弹出层
      dialogVisible: false,

      formLabelWidth: "80px",

      form: {
        name: "",
      },
    };
  },
  methods: {
    query() {},

    reset() {},

    add() {
      this.dialogFormVisible = true;
    },

    //修改
    revamp(id) {
      this.dialogVisible = true;
      console.log(id);
    },

    //分页
    handleSizeChange(val) {
      this.page.pageSize = val;
      console.log(`每页 ${val} 条`);
      this.getList();
    },
    handleCurrentChange(val) {
      this.page.pageNo = val;
      console.log(`当前页: ${val}`);
      this.getList();
    },
    //获取列表数据
    getList() {
      this.dataTotal = testJson;
      this.rows = this.dataTotal.length;
      let { pageNo, pageSize } = this.page;
      this.dataForm = this.dataTotal.slice(
        (pageNo - 1) * pageSize,
        pageNo * pageSize
      );
    },
  },
  mounted() {
    this.getList();
  },
};
</script>

<style></style>
