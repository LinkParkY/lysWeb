<template>
    <div style="width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">

        <div class="pageBox" style="overflow-y: auto">
            <div class="myNavigation">
                <el-breadcrumb separator="/" style="display: inline-block">
                    <el-breadcrumb-item>
                        <span style="font-size: 16px;color:#303313;">报警管理</span>
                    </el-breadcrumb-item>
                    <el-breadcrumb-item>
                        <span style="font-size: 14px;color:#606266;">报警列表</span>
                    </el-breadcrumb-item>
                </el-breadcrumb>
            </div>

            <div style=" height:50px; margin:5px 16px 0 16px;">

                <!--项目下拉框-->
                <div style="float: left; height:40px; width:135px;">
                    <div style="width:135px;">
                        <el-select v-model="projectSelectVal" placeholder="项目" @change="getCenterControl">
                            <el-option
                                label="不限"
                                value="0">
                            </el-option>
                            <el-option
                                v-for="item in projectSelect"
                                :key="item.siteId"
                                :label="item.siteLabel"
                                :value="item.siteId">
                            </el-option>
                        </el-select>
                    </div>
                </div>

                <!--中控下拉框-->
                <div style="float: left; height:40px; width:135px; margin-left: 8px">
                    <div style="width:135px;">
                        <el-select v-model="centerControlVal" placeholder="中控">
                            <el-option
                                label="不限"
                                value="0">
                            </el-option>
                            <el-option
                                v-for="item in centerControlSel"
                                :key="item.id"
                                :label="item.efmLabel"
                                :value="item.id">
                            </el-option>
                        </el-select>
                    </div>
                </div>

                <!--<div style="width:135px; float: left; height:40px;margin-left: 8px">-->
                    <!--<div style="width:135px;">-->
                        <!--<el-select v-model="equipmentTypeValue" placeholder="选择设备类型">-->
                            <!--<el-option-->
                                <!--v-for="item in equipmentType"-->
                                <!--:key="item.id"-->
                                <!--:label="item.name"-->
                                <!--:value="item.id">-->
                            <!--</el-option>-->
                        <!--</el-select>-->
                    <!--</div>-->
                <!--</div>-->

                <!--条件输入框-->
                <div style="width:135px; float: left; height:40px;margin-left: 8px">
                    <!--编号输入框-->
                    <div style="width:135px;">
                        <el-input v-model="inputBoxValue" placeholder="请输入关键字"></el-input>
                    </div>
                </div>

                <div style="float: left; height:40px; width:56px;margin-left: 10px">
                    <el-button class="v-1-0-0-Button" @click="common.page = 1;getAlarmLog()">查询</el-button>
                </div>

                <!--<div style="float: left; height:40px; width:56px;margin-left: 10px">-->
                    <!--<el-button class="v-1-0-0-Button">高级搜索</el-button>-->
                <!--</div>-->

                <div style="float: right;">
                    <el-button class="v-1-0-0-Button" @click="exportAlarm">数据导出</el-button>
                </div>

            </div>

            <div style="height:calc(100% - 152px);margin: 0 16px auto 16px;
            border: 1px solid #F0F2F5;overflow: auto">
                <el-table ref="multipleTable" height="100%" :data="alarmLogData" tooltip-effect="dark" style="width: 100%"
                          :header-cell-style="tableStyle">
                    <el-table-column type="index" label="#" width="56"></el-table-column>

                    <el-table-column prop="siteLabel" label="项目" :formatter="formatProject"></el-table-column>

                    <el-table-column prop="deviceName" label="中控" :formatter="formatNiname" show-overflow-tooltip></el-table-column>

                    <el-table-column prop="serialNumber" :formatter="formatNumber" label="编号" width="70"></el-table-column>

                    <el-table-column prop="message" :formatter="formatMessage" label="地址" show-overflow-tooltip>
                    </el-table-column>

                    <el-table-column prop="wtime" label="报警时间" :formatter="getLocalTime" show-overflow-tooltip></el-table-column>

                    <el-table-column prop="protocolNode" label="报警类型" show-overflow-tooltip></el-table-column>
                    <el-table-column prop="isDeal" label="处理状态" :formatter="formatIsDeal" show-overflow-tooltip></el-table-column>
                    <el-table-column prop="type" label="设备类型" :formatter="formatType" show-overflow-tooltip></el-table-column>

                    <el-table-column fixed="right" label="操作">
                        <template slot-scope="scope">
                            <el-button @click="clearVoice(scope.row)" type="text" size="small">
                                <span style="color: #387EE8;">消音</span>
                            </el-button>

                            <el-button @click.native.prevent="alarmDetail(scope.row)" type="text" size="small">
                                <span style="color: #387EE8;">查看</span>
                            </el-button>

                            <el-button style="color: red" @click.native.prevent="deleteWarning(scope.row)" type="text" size="small">
                                删除
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
                    :total="common.total">
                </el-pagination>
            </div>

        </div>

    </div>
</template>

<script>
    import http from '../../../common/http';
    import bus from '../../../common/bus';
    export default {
        name: 'alarmLog',
        data() {
            return {
                //搜索条件输入框model
                inputBoxValue:"",
                //中控下拉框mdoel
                centerControlVal:null,
                //项目下拉框model
                projectSelectVal:null,
                //设备类型下拉框model
                equipmentTypeValue: null,
                //中控下拉框
                centerControlSel:[],
                //项目下拉框
                projectSelect:[],
                //设备类型下拉框
                equipmentType: [
                    {name: '不限', id: -1},
                    {name: '电弧探测器', id: 0},
                    {name: '组合式探测器', id: 1},
                ],
                tableStyle: {
                    'background-color': '#F6F7FB',
                    'font-weight': '400'
                },
                common: {
                    total: 0,            //总数据量
                    pageSize: 20,        //每页条数
                    page: 1,              //页码
                    isPage: 1,            //是否分页
                    allSizes: [10, 20, 30], //每页条数筛选
                    checkboxArr: []       //多选框数组
                },
                alarmLogData: [],
                filters:{
                    deviceType:1,
                },
                delWarningId:0,
                warnIndex:0,
                start_date: "",
                end_date: "",
                checkEquipmentType: true,
                checkShowEquip: false,
                checkShowCenter: true,
                timeOut: '',
                routeParam:{},
            };
        },

        created: function () {
            this.routeParam = this.$route.params;
            this.inputBoxValue=this.routeParam.inputBoxValue==null ?"":this.routeParam.inputBoxValue;
            this.projectSelectVal=this.routeParam.siteId==null?"0":this.routeParam.siteId;
            this.centerControlVal=this.routeParam.deviceId==null?"0":this.routeParam.deviceId;
            this.common.page=this.routeParam.currentPage==null?1:this.routeParam.currentPage;
            this.common.pageSize=this.routeParam.pageSize==null?20:this.routeParam.pageSize;
            this.getAlarmLog();
            this.getProjectSel();
            let that = this;
            this.timeOut = setInterval(function(){
                that.setTimeFn()
            },30000)
        },
        methods: {
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
            //通过项目获取中控
            getCenterControl(){
                this.centerControlVal = null;
                let url = this.getDeviceBySiteUrl;
                let data = {
                    siteId : this.projectSelectVal
                };
                this.apiPost(url, data).then((res) => {
                    this.centerControlSel =  res.data;
                    for (let i in this.centerControlSel){
                        if(this.centerControlSel[i].niName){
                            this.centerControlSel[i].efmLabel = this.centerControlSel[i].niName;
                        }
                        else if(this.centerControlSel[i].specificator){
                            this.centerControlSel[i].efmLabel = this.centerControlSel[i].specificator;
                        }
                        else{
                            this.centerControlSel[i].efmLabel = parseInt(this.centerControlSel[i].device_code,16);
                        }
                    }
                }, (err) => {

                });
            },

            getAlarmLog() {
                let _loading = this.$commonFn.showLoading(2, '.main-box');
                let url = this.getAlarmLogUrl;
                let data = {
                    isDevice: this.filters.deviceType,//1->终端,0->中控
                    siteId: this.projectSelectVal == null?0:this.projectSelectVal,
                    deviceId: this.centerControlVal == null?0:this.centerControlVal,
                    equipId: 0,
                    inquir: this.inputBoxValue,
                    currentPage: this.common.page,
                    pageSize: this.common.pageSize,
                    type: this.equipmentTypeValue == null?-1:this.equipmentTypeValue,
                    warnIndex: this.warnIndex
                };
                this.apiPost(url, data).then((res) => {
                    _loading.close();
                    console.log(res.data)
                    if (res.status === 200) {
                        this.alarmLogData = res.data.alarmLogForDevice;
                        this.common.total = res.data.alarmLogForDeviceCount;
                    }
                    else{
                        this.$commonFn.showTip(res.message,3);
                    }
                }, (err) => {
                    _loading.close();
                });
            },
            setTimeFn() {
                let url = this.getAlarmLogUrl;
                let data = {
                    isDevice: this.filters.deviceType,//1->终端,0->中控
                    siteId: this.projectSelectVal == null?0:this.projectSelectVal,
                    deviceId: this.centerControlVal == null?0:this.centerControlVal,
                    equipId: 0,
                    inquir: this.inputBoxValue,
                    currentPage: this.common.page,
                    pageSize: this.common.pageSize,
                    type: this.equipmentTypeValue == null?-1:this.equipmentTypeValue,
                    warnIndex: this.warnIndex
                };
                this.apiPost(url, data).then((res) => {
                    console.log(res.data)
                    if (res.status === 200) {
                        this.alarmLogData = res.data.alarmLogForDevice;
                        this.common.total = res.data.alarmLogForDeviceCount;
                    }
                    else{
                        this.$commonFn.showTip(res.message,3);
                    }
                }, (err) => {
                    console.log(err)
                });
            },
            formatProject(row){
                if(row.siteName){
                    return row.siteLabel = row.siteName;
                }
                else{
                    return row.siteLabel = row.siteCode;
                }
            },
            formatMessage(row) {
                let address="";
                if(row.message){
                    address= row.message;
                }else if(row.specificator){
                    address= row.specificator;
                }

                return address;
            },
            formatNiname(row){
                if(row.niName){
                    return row.deviceName = row.niName;
                }
                else if(row.specificator){
                    return row.deviceName = row.specificator;
                }
                else{
                    return row.deviceName = parseInt(row.device_code,16);
                }
            },
            formatNumber: function (row, column) {
                return row.serialNumber = parseInt(row.lineCode) + 1 + "-" + row.addr;
            },
            formatType: function (row, column) {
                return row.type == 0 ? "电弧探测器" : "组合式探测器";
            },
            formatIsDeal: function (row) {
                return row.isDeal == 0 ? "未处理" : "待审核";
            },
            //时间戳转时间
            getLocalTime(row) {
                return row.wtime = this.$commonFn.formatDate(row.warning_time);
            },
            //导出报警
            exportAlarm() {
                if(this.alarmLogData.length == 0){
                    this.$commonFn.showTip("无报警记录",2);
                    return;
                }
                let url = this.excelForWarnLogUrl;
                let data = {
                    isDevice: this.filters.deviceType,
                    siteId: (this.projectSelectVal == null?0:this.projectSelectVal),
                    deviceId: (this.centerControlVal == null?"":this.centerControlVal),
                    inquir: this.inputBoxValue == undefined?"":this.inputBoxValue,
                    type: -1,
                    warnIndex:this.warnIndex
                };
                this.apiPost(url, data, 'blob').then((res) => {
                    this.excelForWarnLogSuccess(res);
                }, (err) => {

                });
            },
            //创建excel对象
            excelForWarnLogSuccess(res) {
                const link = document.createElement('a');
                let blob = new Blob([res], {type: 'application/vnd.ms-excel'});
                link.style.display = 'none';
                link.href = URL.createObjectURL(blob);
                let num = '';
                for (let i = 0; i < 10; i++) {
                    num += Math.ceil(Math.random() * 10)
                }
                link.setAttribute('download', '设备报警记录表.xlsx');
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            },
            //查看
            alarmDetail(rows) {
                console.log(rows);
                this.$router.push(
                    {
                        name: 'alaLogDev',
                        params:{
                            id:rows.warning_no,
                            siteName:rows.siteName?rows.siteName:rows.site_code,
                            niName:rows.niName?rows.niName:rows.device_code,
                            serialNumber:rows.serialNumber,
                            tempration:rows.tempration,
                            protocolNode:rows.protocolNode,
                            isDeal:rows.isDeal,
                            warnTime:rows.wtime,
                            inputBoxValue: this.inputBoxValue,
                            currentPage: this.common.page,
                            pageSize: this.common.pageSize,
                            siteId: this.projectSelectVal,
                            deviceId: this.centerControlVal,
                        }
                    }
                );
            },

            clearVoice(rows) {
                if (rows.etype == 2) {
                    let url = this.sendAdminSocketUrl;
                    let data = {
                        protocolName: 'udp_clear_voice',
                        siteCode: rows.siteCode,
                        deviceCode: rows.deviceCode,
                        extraData: rows.eid.substring(rows.eid.length - 4)
                    };
                    console.log(data)
                    this.apiPost(url, data).then((res) => {
                        if(res.status === 200){
                            localStorage.setItem('cmd',"udp_clear_voice");
                            localStorage.setItem('efmId',rows.siteCode+rows.deviceCode);
                            let that = this;
                            this.$constant.setCmdTime = setTimeout(function () {
                                that.apiPost(url, data).then((res) => {
                                    if(res.status === 200){
                                        let self = that;
                                        that.$constant.setCmdTime = setTimeout(function () {
                                            localStorage.removeItem('cmd');
                                            localStorage.removeItem('efmId');
                                            that.$commonFn.showTip("消音超时", 2);
                                        },4000)
                                    }
                                    else{
                                        that.$commonFn.showTip(res.message,3);
                                    }
                                }, (err) => {
                                    console.log(err);
                                });
                            },4000)
                        }
                        else{
                            this.$commonFn.showTip(res.message,3);
                        }
                    }, (err) => {
                        console.log(err);
                    });
                }
                else if (rows.etype == 0) {
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'alarm_silencing',
                        qos: 1,
                        ID: "01"+rows.eid,
                        type: 3,
                        extraData:""
                    };
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("消音发送成功", 1);
                    }, (err) => {
                        console.log(err);
                    });

                }
            },

            //删除报警
            deleteWarning(row){
                this.delWarningId = row.warning_no;
                this.$commonFn.showConfirm(this.deleteSuccess);
            },
            deleteSuccess(){
                let url = this.delWarningUrl;
                let data = {
                    warnId: this.delWarningId
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.$commonFn.showTip("删除成功",1);
                        if(this.common.total % this.common.pageSize == 1){
                            if(this.common.page == 1){
                                this.common.page == 1
                            }else{
                                this.common.page -= 1
                            }
                        }
                        bus.$emit("updateAlarmNum");
                        this.getAlarmLog();
                        let alarmCount = parseInt(localStorage.getItem('alarmCount'));
                        alarmCount -= 1;
                        localStorage.setItem('alarmCount',alarmCount);
                    }
                    else{
                        this.$commonFn.showTip(res.message,3);
                    }
                }, (err) => {

                });
            },
            handleSizeChange(val) {
                this.common.pageSize = val;
                this.common.page = 1 ;
                this.getAlarmLog();
            },
            handleCurrentChange(val) {
                this.common.page = val ;
                this.getAlarmLog();
            }
        },
        destroyed(){
            clearInterval(this.timeOut)
        },
        mixins: [http]
    };
</script>

<style scoped>


</style>

