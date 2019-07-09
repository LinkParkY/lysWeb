<template>
  <div style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">
    <div class="pageBox">
      <div class="myNavigation">
        <el-breadcrumb separator="/" style="display: inline-block">
          <el-breadcrumb-item>
            <span style="font-size: 16px;color:#303313;">系统管理</span>
          </el-breadcrumb-item>
          <el-breadcrumb-item>
            <span style="color: #111111;font-size: 14px;color:#606266;">扫码上报</span>
          </el-breadcrumb-item>
        </el-breadcrumb>
      </div>

      <div style=" height:50px; margin:5px 16px 0 16px;">
        <div class="flTitle">项目名称</div>
        <div class="fl">
          <el-input v-model="siteNameVal" style="width:130px" placeholder="请输入项目名称"></el-input>
        </div>
<!--        <div class="flTitle">设备名称</div>-->
<!--        <div class="block fl">-->
<!--          <el-input v-model="deviceNameVal" style="width:130px" placeholder="请输入设备名称"></el-input>-->
<!--        </div>-->
        <div class="flTitle">ICCID</div>
        <div class="fl">
          <el-input v-model="deviceCodeVal" style="width:130px" placeholder="请输入ICCID"></el-input>
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
          <el-table-column prop="siteName" label="项目名称" show-overflow-tooltip width="120"></el-table-column>

          <el-table-column  label="编号" width="200">
              <template slot-scope="scope">
                  <span v-if="scope.row.deviceCode==null || scope.row.lineCode==null || scope.row.addr==null">设备已删除</span>
                  <span v-if="scope.row.deviceCode!=null && scope.row.lineCode!=null && scope.row.addr!=null">{{parseInt(scope.row.deviceCode, 16) + '-' + (parseInt(scope.row.lineCode) + 1) + '-' + scope.row.addr}}</span>
              </template>
          </el-table-column>

              <el-table-column label="设备类型" show-overflow-tooltip>
                  <template slot-scope="scope">
                      <el-tag v-if="scope.row.deviceType == 0">电弧探测器</el-tag>
                      <el-tag type="warning" v-if="scope.row.deviceType == 1">组合式探测器</el-tag>
                  </template>
              </el-table-column>

          <el-table-column prop="nbChipSerialNumber" label="ICCID" show-overflow-tooltip></el-table-column>
          <el-table-column prop="createTime" label="创建时间" show-overflow-tooltip></el-table-column>
          <el-table-column fixed="right" label="操作" width="160">
            <template slot-scope="scope">
              <el-button
                @click.native.prevent="queryResult(scope.$index, scope)"
                type="text"
                size="small"
              >
                <span style="color: #387EE8;">查看</span>
              </el-button>
              <el-button
                @click.native.prevent="deleteRow(scope.$index, scope)"
                type="text"
                size="small"
              >
                <span style="color: #387EE8;">删除</span>
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
      
      deviceNameVal: "",
      developTypeValue: '',
      developType: [
        { name: "全部", id: -1 },
        { name: "未升级", id: 0 },
        { name: "升级中", id: 1 },
        { name: "已升级", id: 2 }
      ],
      siteNameVal: "",
      deviceCodeVal: "",
      delId: 0
    };
  },
  created: function() {
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
      let url = this.qrCodeUrl;
      let getParams = {
        page: this.common.page,
        limit: this.common.pageSize,
      };
      if(this.deviceCodeVal != ''){
        getParams.deviceCode = this.deviceCodeVal
      }
      if (this.siteNameVal != "") {
        getParams.siteName = this.siteNameVal;
      }
      if (this.deviceNameVal != "") {
        getParams.deviceName = this.deviceNameVal;
      }
      console.log(getParams);
      this.$axios.post(url, getParams).then(res => {
          this.common.total = res.data.data.count;
          this.upgradeList = res.data.data.list;
        });
    },
    //查看二维码结果
    queryResult(index, rows) {
      console.log(rows)
      // if(rows.row.deviceCode == null){
      //   this.$commonFn.showTip('deviceCode为空', 3);
      //   return;
      // }
      this.$router.push({
        name: "ScanReportDetail",
        params: {
            ICCID: rows.row.nbChipSerialNumber,
            siteName:rows.row.siteName,
            lineCode:rows.row.lineCode,
            addr:rows.row.addr,
            deviceCode:rows.row.deviceCode,
            url:rows.row.url,
            deviceType:rows.row.deviceType,

        }
      });
    },
    // 请求数据
    getUpgradeList() {
      let url = this.qrCodeUrl;
      let data = {
        page: this.common.page,
        limit: this.common.pageSize,
      };
      console.log(data)
       this.$axios.post(url, data).then(res => {
          console.log(res.data)
          this.common.total = res.data.data.count;
          this.upgradeList = res.data.data.list;
        });
    },
    
    
    //删除
    deleteRow(index, rows) {
      //rows.splice(index, 1);
      console.log(rows)
      this.delId = rows.row.id;
      this.$commonFn.showConfirm(this.deleteSuccess);
    },
    // 新增
    newAdd() {
      this.$router.push({
        name: "AddScanReport",
        params: {
          type : 1
        }
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
      let url = this.delQRCode;
      let data = {
        id: this.delId
      };
      console.log(data);
      this.$axios.post(url, data).then(
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
      let url = this.qrCodeUrl;
      let data = {
        page: this.common.page,
        limit: val,
      };
       this.$axios.post(url, data).then(res => {
          this.upgradeList = res.data.data.list;
        });
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      let url = this.qrCodeUrl;
      let data = {
        page: val,
        limit: this.common.pageSize,
      };
       this.$axios.post(url, data).then(res => {
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

