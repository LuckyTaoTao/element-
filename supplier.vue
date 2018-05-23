<template>
  <div class="goodsattribute">
    <div id="leixin">
      <h3 class="title_container">供应商管理</h3>
      <div class="search_container">
        <el-form :inline="true" :model="goodsattribute" class="demo-form-inline">
          <el-form-item label="供应商(厂家)名称" size="mini">
            <el-input v-model="goodsattribute.supplier_name"></el-input>
          </el-form-item>
          <el-form-item label="供应商(厂家)编号" size="mini">
            <el-input v-model="goodsattribute.supplier_id"></el-input>
          </el-form-item>
          <el-form-item label="助记码" size="mini">
            <el-input v-model="goodsattribute.logogram"></el-input>
          </el-form-item>
          <el-form-item label="公司电话" size="mini">
            <el-input v-model="goodsattribute.company_tel"></el-input>
          </el-form-item>
          <el-form-item size="mini">
            <el-button plain @click="onSearch" icon="el-icon-search" size="mini">查询</el-button>
          </el-form-item>
        </el-form>
      </div>
      <div class="add_delet">
        <el-button plain icon="el-icon-circle-plus-outline" @click="dialogFormVisible = true" class="addBtn" size="mini">新增</el-button>
        <el-button plain icon="el-icon-delete" @click="deletes" size="mini">删除</el-button>
      </div>
      <div class="table_container">
        <el-table :data="TableData.tableData" style="width: 100%" @selection-change="handleSelectionChange">
          <el-table-column type="selection" width="50">
          </el-table-column>
          <el-table-column property="registe_time" label="操作">
            <template slot-scope="scope">
              <el-button @click="handleEdit(scope, scope.row)" type="text" size="small">
                修改
              </el-button>
            </template>
          </el-table-column>
          <el-table-column property="supplier_name" label="供应商(厂家)名称" width="200">
          </el-table-column>
          <el-table-column property="logogram" label="助记码">
          </el-table-column>
          <el-table-column property="principal_name" label="负责人姓名" width="100">
          </el-table-column>
          <el-table-column property="company_tel" label="公司电话" width="200">
          </el-table-column>
          <el-table-column property="company_bank" label="开户银行" width="200">
          </el-table-column>
          <el-table-column property="bank_account" label="银行账户" width="200">
          </el-table-column>
          <el-table-column property="bank_account_name" label="开户名称" width="200">
          </el-table-column>
          <el-table-column property="province_name" label="省">
          </el-table-column>
          <el-table-column property="city_name" label="市">
          </el-table-column>
          <el-table-column property="county_name" label="区县">
          </el-table-column>
          <el-table-column property="address" label="地址" width="200">
          </el-table-column>
          <el-table-column property="principal_mobile" label="负责人手机号" width="200">
          </el-table-column>
          <el-table-column property="is_disabled" label="停用标志">
          </el-table-column>
          <el-table-column property="remark" label="备注">
          </el-table-column>
        </el-table>
      </div>
      <div class="page_container">
        <div class="pageNumber">每页显示</div>
        <el-pagination class="erp-pagination" @size-change="handleSizeChange" @current-change="handleCurrentChange" :page-sizes="TableData.per_page" layout="sizes, prev, pager, next" prevText="上一页" nextText="下一页" :total="TableData.total">
        </el-pagination>
      </div>

      <el-dialog title="添加往来帐类型" @open="open" center :visible.sync="dialogFormVisible" width="1000px">
        <el-form :model="ruleForm" ref="ruleForm" :rules='rules'>
          <el-row>
            <el-col :span="12">
              <el-form-item label="供应商(厂家)名称" prop="supplier_name" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.supplier_name" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="负责人姓名" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.principal_name" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="负责人办公电话" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.principal_tel" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="公司电话" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.company_tel" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="开户银行" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.company_bank" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="负责人手机号" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.principal_mobile" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="开户名称" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.bank_account_name" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="银行账户" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.bank_account" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="地址" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.address" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="传真" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.company_fax" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="区域" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <!-- <el-cascader :options="options" v-model="selectedOptions" @change="handleChange">
                </el-cascader> -->
                <el-cascader :options="options2" @active-item-change="handleItemChange" :props="props"></el-cascader>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="电子邮箱" prop="email" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.email" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="备注" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-input v-model="ruleForm.remark" auto-complete="off"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="停用操作" :label-width="formLabelWidth" class="el-form-item_shortInput">
                <el-radio-group v-model="ruleForm.is_disabled">
                  <el-radio :label="1">是</el-radio>
                  <el-radio :label="0">否</el-radio>
                </el-radio-group>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="dialogFormsubmit('ruleForm')">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import { regionData } from "element-china-area-data";
export default {
  data() {
    return {
      options: regionData,
      selectedOptions: [],
      TableData: {
        tableData: [],
        per_page: [10],
        total: 10
      },
      goodsattribute: {
        type_id: "",
        type_name: "",
        logogram: ""
      },
      options2:[],
      props: {
          value: 'value',
          children: 'cities'
      },
      multipleSelection: [], //已选项
      formLabelWidth: "130px",
      dialogFormVisible: false,
      ruleForm: {},
      rules: {
        supplier_name: [
          { required: true, message: "请输入供应商名称", trigger: "blur" }
        ],
        email: [
          { message: "请输入邮箱地址", trigger: "blur" },
          {
            type: "email",
            message: "请输入正确的邮箱地址",
            trigger: ["blur", "change"]
          }
        ],
        usernamber: [
          { required: true, message: "不能为空" },
          { type: "number", message: "只能输入数字" }
        ]
      }
    };
  },
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val;
      console.log(this.multipleSelection);
    },
    handleEdit(index, row) {
      this.dialogFormVisible = true;
      console.log(row);
      this.$nextTick(function() {
        this.ruleForm = row;
      });
    },
    onSearch() {
      //查询
      this.$common.initData(
        "getSupplierList",
        this.TableData,
        this.goodsattribute
      );
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      let page = {
        page: val
      };
      this.$common.handleCurrentChange("getSupplierList", page, this.TableData);
    },
    dialogFormsubmit(ruleForm) {
      var This = this;
      this.$refs[ruleForm].validate(valid => {
        this.ruleForm.province_id = This.selectedOptions[0];
        this.ruleForm.city_id = This.selectedOptions[1];
        this.ruleForm.county_id = This.selectedOptions[2];
        if (valid) {
          if (This.ruleForm.type_id == undefined) {
            let funcan = {
              eachData: this.ruleForm,
              UrlApi: "addSupplier",
              closeDialog: (this.dialogFormVisible = false),
              list: {
                UrlApi: "getSupplierList",
                data: this.TableData
              }
            };
            this.$common.addAndupdateFun(funcan);
          } else {
            let funcan = {
              eachData: this.ruleForm,
              UrlApi: "editSupplier",
              closeDialog: (this.dialogFormVisible = false),
              list: {
                UrlApi: "getSupplierList",
                data: this.TableData
              }
            };
            this.$common.addAndupdateFun(funcan);
          }
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    handleChange(val) {
      console.log(val);
    },
    deletes() {
      var deledata = {
        deleID: "supplier_id",
        deleSelect: this.multipleSelection,
        deleApi: "delSupplier",
        list: {
          UrlApi: "getSupplierList",
          data: this.TableData
        }
      };
      this.$common.deletes(deledata);
    },
    open() {
      this.ruleForm = {};
    },
    handleItemChange(val){
      var p_id;
      if(val.length == 1){
         p_id = val[0];
      }
      if(val.length == 2){
         p_id = val[1]
      }
      this.$axios({
        method: "get",
        url: this.$api.getAreaList,
        params: {p_id:p_id,is_page:1}
      }).then(res => {
        for(var i in this.options2){
          if(val.length == 1 && this.options2[i].value == val[0] && !this.options2[i].cities.length){
            for( var j in res.data.data){
              var cities = {};
              cities.label=res.data.data[j].area_name
              cities.value=res.data.data[j].area_id
              cities.cities=[]

              this.options2[i].cities.push(cities);
            }

            
          }

          if(val.length == 2){
            for(var k in this.options2[i].cities){
              console.log(this.options2[i].cities[k]);
              if(this.options2[i].cities[k].value == val[1] && !this.options2[i].cities[k].cities.length){
                for( var j in res.data.data){
                  var cities = {};
                  cities.label=res.data.data[j].area_name
                  cities.value=res.data.data[j].area_id

                  this.options2[i].cities[k].cities.push(cities);
                }
              }
            }
            
          }
          
        }
      });
    }
  },
  mounted() {
    this.$common.initData("getSupplierList", this.TableData);
      this.$axios({
        method: "get",
        url: this.$api.getAreaList,
        params: {level:2,is_page:1}
      }).then(res => {
           var province={}
        // console.log(res.data.data)
        // // this.attribute = res.data.data;
        for(var i in res.data.data){
          var province={}
          province.label=res.data.data[i].area_name
          province.value=res.data.data[i].area_id
          province.cities=[]
          this.options2.push(province);
        //   {
        //   label: '江苏',
        //   cities: []
        // }
        }
        // this.options2.push(province);
        console.log(this.options2)
      });
  }
};
</script>

<style scoped>
.el-pagination__total {
  padding: 2px 5px;
  line-height: 28px;
  display: inline-block;
  width: 6%;
  vertical-align: middle;
}
.page_container {
  margin-top: 40px;
}
.add_delet {
  margin-bottom: 10px;
}
.el-pagination {
  display: inline-block;
  width: 90%;
  text-align: right;
}
.pageNumber {
  float: left;
  width: 56px;
  height: 36px;
  line-height: 36px;
  font-size: 14px;
  color: #949495;
}
</style>