<template>
  <div style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">
    <div class="pageBox">
      <div class="myNavigation">
        <el-breadcrumb separator="/" style="display: inline-block">
          <el-breadcrumb-item>
            <span style="font-size: 16px;color:#303313;">升级结果</span>
          </el-breadcrumb-item>
          <!-- <el-breadcrumb-item>
                        <span style="font-size: 14px;color:#606266;">无线终端列表</span>
          </el-breadcrumb-item>-->
        </el-breadcrumb>
        <el-button type="primary" @click="cancel()" style="float:right">返回</el-button>
      </div>
      <div
        style="height:calc(100% - 152px);margin: 0 16px auto 16px;border: 1px solid #F0F2F5;overflow: auto"
      >
        <el-table
          :data="resultList"
          border
          style="width: 100%;color:#000;"
          tooltip-effect="dark"
          :headerCellStyle="tableStyle"
        >
          <el-table-column label="设备编号">
            <template slot-scope="scope">
                <span v-if="scope.row.deviceCode==null || scope.row.lineCode==null || scope.row.addr==null">设备已删除</span>
                <span v-if="scope.row.deviceCode!=null && scope.row.lineCode!=null && scope.row.addr!=null">{{parseInt(scope.row.deviceCode, 16) + '-' + (parseInt(scope.row.lineCode) + 1) + '-' + scope.row.addr}}</span>
            </template>
          </el-table-column>
          <el-table-column label="设备类型">
            <!-- prop="deviceType" -->
            <template slot-scope="scope">
              <!-- <span v-if="scope.row.deviceType == 0">无线</span>
              <span v-if="scope.row.deviceType == 1">有线</span> -->
              <el-tag v-if="scope.row.deviceType == 0">无线</el-tag>
              <el-tag type="warning" v-if="scope.row.deviceType == 1">有线</el-tag>
            </template>
          </el-table-column>
          <el-table-column prop="ipAddr" label="IP地址"></el-table-column>
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
          <el-table-column prop="requestTime" label="请求时间" show-overflow-tooltip></el-table-column>
          <el-table-column prop="responseTime" label="响应时间" show-overflow-tooltip></el-table-column>
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
  name: "upgrade",
  data() {
    return {
      resultList: [],
      routeParam: {},
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
      }
    };
  },
  /*方法体*/
  created: function() {
    this.routeParam = this.$route.params;
    this.getDetail();
  },
  methods: {
    selectEquipment() {
      this.dialogVisible = true;
    },
    getCheckedKeys() {
      console.log(this.data);
      console.log(this.$refs.tree.getCheckedNodes());
    },
    // 获取数据
    getDetail() {
      let url = this.detailListUrl;
      let data = {
        // batchNum: this.inputVal,
        // saveTime: this.timeVal,
        // upgradeType: this.developTypeValue
        page: this.common.page,
        limit: this.common.pageSize,
        mainId: this.routeParam.mianId


      };

      // this.apiGet(url, data).then(res => {
      //   console.log(res.data);
      //   this.common.total = res.data.total;
      //   this.resultList = res.data.list;
      // });
        this.$axios
            .get(url, {
                params: data
            })
            .then(res => {
                console.log(res.data)
                  this.common.total = res.data.data.total;
                  this.resultList = res.data.data.list;
            });
    },
    cancel() {
      this.$router.go(-1);
    },
    save() {
      this.$router.go(-1);
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.common.pageSize=val;
      let url = this.detailListUrl;
      let data = {
        page: this.common.page,
        limit: val,
        mainId: this.routeParam.mianId
      };
      this.$axios
        .get(url, {
          params: data
        })
        .then(res => {
          this.resultList = res.data.data.list;
        });
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      let url = this.detailListUrl;
      let data = {
        page: val,
        limit: this.common.pageSize,
        mainId: this.routeParam.mianId
      };
       this.$axios
        .get(url, {
          params: data
        })
        .then(res => {
          this.resultList = res.data.data.list;
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
}
.myNavigation {
  height: 50px;
  line-height: 50px;
  margin-right: 16px;
}
</style>


