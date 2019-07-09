<template>
<div style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">

    <div class="pageBox">
        <div class="myNavigation">
            <el-breadcrumb separator="/" style="display: inline-block">
                <el-breadcrumb-item>
                    <span style="font-size: 16px;color:#303313;">报警管理</span>
                </el-breadcrumb-item>
                <el-breadcrumb-item>
                    <span style="color: #111111;font-size: 14px;color:#606266;">处理日志</span>
                </el-breadcrumb-item>
            </el-breadcrumb>
        </div>

        <div style="height:50px; margin:5px 16px 0 16px;" >
            <!--项目下拉框-->
            <div style="float: left; height:40px; width:135px;margin-right: 1rem">
                <div style="width:135px;">
                    <el-select  v-model="projectSelectVal" placeholder="项目" @change="getCenterControl">
                        <el-option
                            label="不限"
                            value="">
                        </el-option>
                        <el-option
                            v-for="item in projectSelect"
                            :key="item.siteId"
                            :label="item.siteLabel"
                            :value="item.siteCord">
                        </el-option>
                    </el-select>

                </div>

            </div>
            <!--类型下拉框-->
            <div style="float: left; height:40px; width:110px;margin-right: 1rem">
                <div style="width:110px;">


                    <el-select  v-model="equipTypeValue" @change="clickEquipType" placeholder="类型">
                        <el-option label="不限" :value="-1">
                        </el-option>
                        <el-option v-for="item in equipmentType" :key="item.value"
                                   :label="item.label" :value="item.value">
                        </el-option>
                    </el-select>
                </div>

            </div>
            <!--条件输入框-->
            <div style="width:135px; float: left; height:40px;margin-right: 1rem">
                <!--编号输入框-->
                <div style="width:135px;">
                    <el-input v-model="inputBoxValue" placeholder="请输入关键字"></el-input>
                </div>
            </div>
            <div style=" float: left; margin-top: 0.1px;">
                <el-date-picker
                    style="width: 135px !important;"
                    v-model="start_date"
                    type="date"
                    format="yyyy-MM-dd"
                    value-format="yyyy-MM-dd"
                    placeholder="起始日期">
                </el-date-picker>

                &nbsp;<span style="font-size: 14px;margin-left: -5px;width: 5px;">至</span>&nbsp;

                <el-date-picker
                    style="width: 135px !important;margin-left: -4px;"
                    v-model="end_date"
                    type="date"
                    format="yyyy-MM-dd"
                    value-format="yyyy-MM-dd"
                    placeholder="结束日期">
                </el-date-picker>
            </div>

            <div style="width:56px; float: left; height:40px; margin-left: 10px; ">
                <el-button class="v-1-0-0-Button" @click="common.page = 1;getAlarmHistory()">查询</el-button>
            </div>

            <div style=" float: right;">
                <el-button class="v-1-0-0-Button" @click="exportHistory">数据导出</el-button>
            </div>

        </div>

        <div style="height:calc(100% - 152px);margin: 0 16px auto 16px;border: 1px solid #F0F2F5;overflow: auto">
            <el-table ref="multipleTable" height="100%" :data="historyList" tooltip-effect="dark" style="width: 100%"
                      @selection-change="" :header-cell-style="tableStyle">
                <el-table-column type="index" width="56" label="#">
                </el-table-column>

                <el-table-column prop="siteLabel"  :formatter="formatProject" label="项目">
                </el-table-column>

                <el-table-column prop="deviceName" label="中控" :formatter="formatNiname" show-overflow-tooltip>
                </el-table-column>
                <el-table-column prop="serialNumber" label="编号"  :formatter="formatNumber" show-overflow-tooltip>
                </el-table-column>

                <el-table-column prop="message" :formatter="formatMessage"  label="地址" show-overflow-tooltip>
                </el-table-column>

                <el-table-column prop="alarmTime" label="报警时间" :formatter="getAlarmTime" show-overflow-tooltip>
                </el-table-column>

                <el-table-column prop="dTime" label="处理时间" :formatter="getLocalTime" show-overflow-tooltip>
                </el-table-column>

                <el-table-column prop="protocolState" :formatter="formatState" label="类型" show-overflow-tooltip>
                </el-table-column>

                <el-table-column fixed="right" label="操作" width="200">
                    <template slot-scope="scope">
                        <el-button style="color: #387EE8;" @click.native.prevent="checkHistory(scope.row)" type="text" size="small">
                            查看
                        </el-button>
                        <el-button style="color: red" @click.native.prevent="deleteHistory(scope.row)" type="text" size="small">
                            删除
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>

        <div style="float: right;margin-right: 13px;" >
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :page-size="common.pageSize"
                :page-sizes="common.allSizes"
                :current-page="common.page"
                layout="total , sizes, prev, pager, next, jumper"
                :total="common.total">
            </el-pagination>
        </div>

    </div>

</div>
</template>

<script>
    import http from '../../../common/http';
    export default {
        data() {
            return {
                tableStyle:{
                    'background-color':'#F6F7FB',
                    'font-weight':'400'
                },
                common:{
                    total:0,            //总数据量
                    pageSize:20,        //每页条数
                    page:1,              //页码
                    isPage:1,            //是否分页
                    allSizes:[10,20,30], //每页条数筛选
                    checkboxArr:[]       //多选框数组
                },
                historyList:[],
                start_date: "",
                end_date: "",
                checkShowEquip:false,
                checkShowCenter:true,
                //输入的编号 equipmentValue
                numberText:"",
                //开始时间
                startTime: "",
                //结束时间
                endTime: "",
                delWarningId:0,
                //项目下拉框model
                projectSelectVal:null,
                //项目下拉框
                projectSelect:[],
                //项目编号
                siteCord: "",
                //终端类型
                equipTypeValue: -1,
                //设备类型数组
                equipmentType: [
                    {
                        label: "电弧",
                        value: "0"
                    },
                    {
                        label: "组合",
                        value: "1"
                    },
                    {
                        label: "中控",
                        value: "2"
                    },
                ],
                inputBoxValue:"",
            };
        },
        created:function(){
            this.getAlarmHistory();
            this.getProjectSel();
        },
        methods: {
            //选择类型
            clickEquipType(){
              console.info("hh",this.equipTypeValue);
            },
            //获取项目下拉框
            getProjectSel(){
                let url = this.getAllGroupUrl;
                let data = {};
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.projectSelect = res.data.getsiteList;
                        for(let i in this.projectSelect){
                            if(this.projectSelect[i].siteName){
                                this.projectSelect[i].siteLabel = this.projectSelect[i].siteName;
                            }
                            else{
                                this.projectSelect[i].siteLabel = this.projectSelect[i].siteCord;
                            }
                        }
                    }
                    else{
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {

                });
            },
            formatMessage(row) {
                return row.message == null?"":row.message;
            },
            //通过项目获取中控
            getCenterControl(){
               console.info("hhh",this.projectSelectVal)
            },
            formatProject(row){
                if(row.siteName){
                    return row.siteLabel = row.siteName;
                }
                else{
                    return row.siteLabel = row.siteCode
                }
            },
            formatNiname(row){
                if(row.niName){
                    return row.deviceName = row.niName;
                }
                if(row.specificator){
                    return row.deviceName = row.specificator;
                }
                else if(row.device_code!=null){
                    return row.deviceName = parseInt(row.device_code,16);
                }else {
                    return row.deviceName = "无线终端";
                }
            },
            formatNumber: function (row) {
                if(row.lineCode && row.addr){
                    return row.serialNumber = parseInt(row.lineCode) + 1 + "-" + row.addr;
                }
                else{
                    return row.serialNumber = "无";
                }
            },
            formatState: function (row){
                let state = row.isReset?"告警复位":"已审核";
                return row.protocolState = row.protocolNode + "/" + state;
            },
            getAlarmHistory(){
                let url = this.alarmHistoryUrl;
                let data = {
                    siteCode:this.projectSelectVal?this.projectSelectVal:"",
                    type : this.equipTypeValue,
                    inquire: this.inputBoxValue,
                    startTime:this.start_date?this.start_date:"",
                    endTime:this.end_date?this.end_date:"",
                    currentPage:this.common.page,
                    pageSize:this.common.pageSize,
                };
                this.apiPost(url, data).then((res) => {
                    this.historyList = res.data.alarmHistoryForAll;
                    this.common.total = res.data.total;
                }, (err) => {

                });
            },
            exportHistory(){
                if(this.historyList.length == 0){
                    this.$commonFn.showTip("无报警历史",2);
                    return;
                }
                let url = this.exportHistoryUrl;
                let data = {
                    startTime: this.start_date,
                    endTime: this.end_date,
                };
                this.apiPost(url,data,'blob').then((res) => {
                    this.excelHistorySuccess(res);
                },(err)=>{

                });
            },
            excelHistorySuccess(res){
                const link = document.createElement('a');
                let blob = new Blob([res], {type: 'application/vnd.ms-excel'});
                link.style.display = 'none';
                link.href = URL.createObjectURL(blob);
                link.setAttribute('download', '报警历史记录表.xlsx');
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            },
            getLocalTime(row) {
                return row.dtime = row.isReset?this.$commonFn.formatDate(row.resetTime):this.$commonFn.formatDate(row.dispatch_time);
            },
            getAlarmTime(row){
                return row.alarmTime=this.$commonFn.formatDate(row.warning_time);
            },
            handleSizeChange(val) {
                this.common.pageSize = val;
                this.common.page = 1 ;
                this.getAlarmHistory();
            },
            handleCurrentChange(val) {
                this.common.page = val ;
                this.getAlarmHistory();
            },
            checkHistory(rows){
                this.$router.push(
                    {
                        name: 'historyLog',
                        params:{
                            id:rows.warning_no,
                            siteName:rows.siteName?rows.siteName:rows.site_code,
                            niName:rows.deviceName,
                            serialNumber:rows.serialNumber,
                            protocolNode:rows.protocolNode,
                        }
                    }
                );
                // console.log(row);
                // let url = this.getWarnInfoUrl;
                // let data = {
                //     warnId: row.warning_no
                // };
                // this.apiPost(url, data).then((res) => {
                //     if (res.status === 200) {
                //         console.log(res);
                //     }
                //     else {
                //         this.$commonFn.showTip(res.message, 3);
                //     }
                // }, (err) => {
                //
                // });
            },
            deleteHistory(row){
                this.delWarningId = row.warning_no;
                this.$commonFn.showConfirm(this.deleteSuccess);
            },
            deleteSuccess() {
                let url = this.delWarningUrl;
                let data = {
                    warnId: this.delWarningId
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.$commonFn.showTip("删除成功", 1);
                        this.common.page = 1;
                        this.getAlarmHistory();
                    }
                    else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {

                });
            },
        },


        mixins: [http]
    };
</script>

<style scoped>

</style>
