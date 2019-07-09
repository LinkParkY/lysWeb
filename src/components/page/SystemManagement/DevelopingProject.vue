<template>
  <div style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">
    <div class="pageBox">
      <div class="myNavigation">
        <el-breadcrumb separator="/" style="display: inline-block">
          <el-breadcrumb-item>
            <span style="font-size: 16px;color:#303313;">系统管理</span>
          </el-breadcrumb-item>
          <el-breadcrumb-item>
            <span style="color: #111111;font-size: 14px;color:#606266;">线上升级</span>
          </el-breadcrumb-item>
        </el-breadcrumb>
      </div>

      <div style=" height:50px; margin:5px 16px 0 16px;">
        <!-- <div style="display: inline-block; height:40px;">
          <el-select v-model="siteList.siteId" placeholder="请选择项目">
            <el-option
              v-for="item in siteList"
              :key="item.siteList.siteId"
              :label="item.siteList.siteName"
              :value="item.siteId"
            ></el-option>
          </el-select>
        </div>
        <div style="display: inline-block; height:40px;float: right;">
          <el-button class="v-1-0-0-Button">
            <i class="okay myIcon-shengji" style="font-size: 15px;">
              <span>升级</span>
            </i>
          </el-button>
        </div>-->
        <div class="flTitle">批次号</div>
        <div class="fl">
          <el-input v-model="inputVal" style="width:130px" placeholder="请输入内容"></el-input>
        </div>
        <div class="flTitle">保存时间</div>
        <div class="block fl">
          <el-date-picker
            v-model="timeVal"
            type="daterange"
            align="right"
            unlink-panels
            range-separator="至"
            start-placeholder="年/月/日"
            end-placeholder="年/月/日"
            :picker-options="pickerOptions2"
            size="small"
            style="width:230px"
          ></el-date-picker>
        </div>
        <div class="flTitle">升级状态</div>
        <div class="fl">
          <el-select
            v-model="developTypeValue"
            @change="select(developTypeValue)"
            style="width:150px;"
            placeholder="选择设备类型"
          >
            <el-option
              v-for="item in developType"
              :key="item.id"
              :label="item.name"
              :value="item.id"
            ></el-option>
          </el-select>
        </div>
        <div>
          <el-button @click.native.prevent="queryData()" type="primary" size="small">
            <span>查询</span>
          </el-button>
          <el-button @click.native.prevent="newAdd()" type="primary" size="small">
            <span>新增</span>
          </el-button>
        </div>
      </div>
      <div
        style="height:calc(100% - 152px);margin: 0 16px auto 16px;border: 1px solid #F0F2F5;overflow: auto"
      >
        <el-table
          :data="upgradeList"
          border
          style="width: 100%;color:#000;"
          tooltip-effect="dark"
          :headerCellStyle="tableStyle"
        >
          =
          <el-table-column prop="bachNum" label="批次号" show-overflow-tooltip width="120"></el-table-column>
          <el-table-column label="设备类型" width="120">
            <template slot-scope="scope">
              <!-- <span v-if="scope.row.deviceType == 0">无线</span>
              <span v-if="scope.row.deviceType == 1">有线</span> -->
              <el-tag v-if="scope.row.deviceType == 0">无线</el-tag>
              <el-tag type="warning" v-if="scope.row.deviceType == 1">有线</el-tag>
            </template>
          </el-table-column>
          <!-- <el-table-column prop="bachNum/deviceType" label="成功数/设备总数" width="200"></el-table-column> -->
          <el-table-column label="成功数/设备总数" width="200">
            <template slot-scope="scope">
              <span>{{scope.row.succeedNum}}/{{scope.row.deviceNum}}</span>
            </template>
          </el-table-column>
          <el-table-column prop="upgradeFile" label="升级文件" show-overflow-tooltip></el-table-column>
          <el-table-column label="升级状态" show-overflow-tooltip>
            <template slot-scope="scope">
              <!-- <span v-if="scope.row.upgradeState == 0">未升级</span>
              <span v-if="scope.row.upgradeState == 1">升级中</span>
              <span v-if="scope.row.upgradeState == 2">升级成功</span>
              <span v-if="scope.row.upgradeState == 3">升级失败</span> -->
              <el-tag type="info" v-if="scope.row.upgradeState == 0">未升级</el-tag>
              <el-tag type="warning" v-if="scope.row.upgradeState == 1">升级中</el-tag>
              <el-tag type="success" v-if="scope.row.upgradeState == 2">升级成功</el-tag>
              <el-tag type="danger" v-if="scope.row.upgradeState == 3">升级失败</el-tag>
            </template>
          </el-table-column>
          <el-table-column prop="createTime" label="保存时间" show-overflow-tooltip></el-table-column>
          <el-table-column prop="upgradeTime" label="升级时间" show-overflow-tooltip></el-table-column>
          <el-table-column prop="memo" label="备注" show-overflow-tooltip></el-table-column>
          <el-table-column fixed="right" label="操作" width="200">
            <template slot-scope="scope">
              <el-button
                v-show="scope.row.upgradeState == 0"
                @click.native.prevent="upgradeRow(scope.$index, scope)"
                type="text"
                size="small"
              >
                <span style="color: #387EE8;">升级</span>
              </el-button>
              <el-button
                v-show="scope.row.upgradeState == 0"
                @click.native.prevent="reviseRow(scope.$index, scope)"
                type="text"
                size="small"
              >
                <span style="color: #387EE8;">修改</span>
              </el-button>
              <el-button
                v-show="scope.row.upgradeState == 0"
                @click.native.prevent="deleteRow(scope.$index, scope)"
                type="text"
                size="small"
              >
                <span style="color: #387EE8;">删除</span>
              </el-button>

              <el-button
                v-show="scope.row.upgradeState !=0"
                @click.native.prevent="queryResult(scope.$index, scope)"
                type="text"
                size="small"
              >
                <span style="color: #387EE8;">查看升级结果</span>
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div style="float: right;margin-right: 13px;">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :page-size="common.pageSize"
          :page-sizes="common.allSizes"
          :current-page="common.page"
          layout="total,sizes, prev, pager, next, jumper"
          :total="common.total"
        ></el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
import http from "../../../common/http.js";
import bus from "../../../common/bus";
export default {
  data() {
    return {
      tableStyle: {
        "background-color": "#F2F2F2",
        // "background": "red",
        color: "#000",
        "font-weight": "400"
      },
      common: {
        total: 0, //总数据量
        pageSize: 10, //每页条数
        page: 1, //页码
        isPage: 1, //是否分页
        allSizes: [10, 20, 30], //每页条数筛选
        checkboxArr: [] //多选框数组
      },
      siteList: [],
      //表格数据
      upgradeList: [],
      multipleSelection: [],
      //日期选择器快捷选项
      pickerOptions2: {
        shortcuts: [
          {
            text: "最近一周",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近一个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit("pick", [start, end]);
            }
          },
          {
            text: "最近三个月",
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit("pick", [start, end]);
            }
          }
        ]
      },
      timeVal: "",
      developTypeValue: null,
      developType: [
        { name: "全部", id: -1 },
        { name: "未升级", id: 0 },
        { name: "升级中", id: 1 },
        { name: "已升级", id: 2 }
      ],
      inputVal: "",
      delId: 0
    };
  },
  created: function() {
    this.developTypeValue = -1;
    this.getUpgradeList();
  },
  methods: {
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    // 查询
    queryData() {
      let url = this.upgradeUrl;
      let getParams;
      if (this.developTypeValue == -1) {
        getParams = {};
      } else {
        getParams = {
          upgradeState: this.developTypeValue
        };
      }
      if (this.inputVal != "") {
        getParams.bachNum = this.inputVal;
      }
      if (this.timeVal != "") {
        getParams.createTime = this.timeVal;
      }
      console.log(this.timeVal);
      this.$axios
        .get(url, {
          params: getParams
        })
        .then(res => {
          this.upgradeList = res.data.data.list;
        });
    },
    //查看升级结果
    queryResult(index, rows) {
      this.$router.push({
        name: "upgrade",
        params: {
          mianId: rows.row.id
        }
      });
    },
    // 请求数据
    getUpgradeList() {
      let url = this.upgradeUrl;
      let data = {
        page: this.common.page,
        limit: this.common.pageSize,
      };
      console.log(data)
      // this.apiGet(url, data).then(res => {
      //   console.log(res.data);
      //   this.upgradeList = res.data;
      // });
       this.$axios
        .get(url, {
          params: data
        })
        .then(res => {
            if(res.data.status==0){
                this.$confirm(
                    res.data.message
                )

            }else{
                this.common.total = res.data.data.total;
                this.upgradeList = res.data.data.list;
            }

        });
    },
    //升级
    upgradeRow(index, rows) {
      console.log(index, rows);
      this.$confirm(
        "请确认是否升级批次号为【" + rows.row.bachNum + "】的设备？",
        "信息确认",
        {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        }
      ).then(() => {
        this.upgradeSuccess(rows.row.id);
      });
    },
    //修改
    reviseRow(index, rows) {
      console.log(index, rows);
      this.$router.push({
        name: "newAdd",
        params: {
          id: rows.row.id,
          deviceType: rows.row.deviceType,
          upgradeFile: rows.row.upgradeFile,
          memo: rows.row.memo,
          type: "1"
        }
      });
    },
    //删除
    deleteRow(index, rows) {
      //rows.splice(index, 1);
      this.delId = rows.row.id;
      this.$commonFn.showConfirm(this.deleteSuccess);
    },
    // 新增
    newAdd() {
      this.$router.push({
        name: "newAdd",
        params: {}
      });
    },
    upgradeSuccess(id) {
      let url = this.upgradeSucUrl;
      let data = {
        id: id
      };
      this.$axios.post(url, data).then(res => {
        console.log(res.data);
        // this.$router.push({
        //       name: "upgrade",
        //       params: {}
        //     });
        let data = res.data;
        if (data.status == 200) {
          if (data.message == "升级指令不可重复发送") {
            this.$commonFn.showTip(data.message, 3);
          } else {
            this.$commonFn.showTip(data.message, 1);
            this.$router.push({
              name: "upgrade",
              params: {}
            });
          }
        } else {
          this.$commonFn.showTip(data.message, 3);
        }
      });
    },
    deleteSuccess(id) {
      let url = this.upgradeDelUrl;
      let data = {
        id: this.delId
      };
      console.log(data);
      this.apiPost(url, data).then(
        res => {
          if (res.status === 200) {
            console.log(res);
            this.$commonFn.showTip("删除成功", 1);
            this.getUpgradeList();
          } else {
            this.$commonFn.showTip(res.message, 3);
          }
        },
        err => {}
      );
    },
    select(type) {
      console.log(type);
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      let url = this.upgradeUrl;
      let data = {
        page: this.common.page,
        limit: val,
      };
       this.$axios
        .get(url, {
          params: data
        })
        .then(res => {
          this.upgradeList = res.data.data.list;
        });
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      let url = this.upgradeUrl;
      let data = {
        page: val,
        limit: this.common.pageSize,
      };
       this.$axios
        .get(url, {
          params: data
        })
        .then(res => {
          this.upgradeList = res.data.data.list;
        });
    }
  },
  mixins: [http]
};
</script>

<style scoped>
.v-1-0-0-Button {
  background-color: #387ee8;
  color: white;
  border: 1px solid #387ee8;
}
.el-table th {
  background-color: #387ee8 !important;
}
.el-table thead {
  color: #000 !important;
}
.fl {
  float: left;
  margin-right: 20px;
}
.flTitle {
  margin-right: 10px;
  float: left;
  height: 32px;
  line-height: 32px;
}
</style>

