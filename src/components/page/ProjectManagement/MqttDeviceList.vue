<template id="mqttDeviceList">
    <div style=" width:calc(100% - 200px); height:100%; background-color: #F0F2F5; overflow:auto;">
        <div style="height:28%;margin: 20px 24px;background-color: #fff">
            <div style="height:100%;margin: 16px 20px;background-color: #fff">
                <div style="height: 25px;width:100%;padding-top: 12px;">
                    <div style="height: 25px;width:100%;padding-top: 0;">
                        <el-breadcrumb separator="/" style="display: inline-block">
                            <el-breadcrumb-item>
                                <span style="font-size: 16px;color:#303313;">项目管理</span>
                            </el-breadcrumb-item>
                            <el-breadcrumb-item>
                                <span style="font-size: 14px;color:#606266;">无线中控信息</span>
                            </el-breadcrumb-item>
                        </el-breadcrumb>
                    </div>
                </div>
                <div style="width: 100%;height: 16%; margin-top: 1%; margin-left: -2px;">
                    <div style="margin-top: 0; float: left;">
                        <i class="okay myIcon-xiangmu" style="font-size: 23px;color: orange;"></i>
                        <span style="font-size: 20px;font-weight: 500;">项目：</span>
<!--                        <span>-->
<!--                            <i class="el-icon-success" style="color: limegreen;position: relative;top: -1px;"></i>-->
<!--                            &lt;!&ndash; <i v-show="efmInfo.isOnline == 0" class="el-icon-warning" style="color: lightgray;position: relative;top: -1px"></i> &ndash;&gt;-->
<!--                        </span>&nbsp;-->
                        <span style="font-size: 20px;font-weight: 500;">无线中控</span>
<!--                        <span style="font-size: 10px;font-weight: 300;color: #2d8cf0; cursor: pointer;"-->
<!--                                @click="centerDialogVisible = true">-->
<!--                    <i class="okay myIcon-xiugai" style="font-size: 15px" @click="editNameBox = true;efmNameInput = '无线中控'"></i>-->
<!--                    </span>-->
                    </div>
<!--                    <div style="float: right;position: relative;left: 2px;">-->
<!--                        <el-button class="v-1-0-0-Button" @click="checkEfm">查看</el-button>-->
<!--                    </div>-->

                </div>

                <div style=" height:52%; margin: 10px 0; ">
                    <table class="gridtable" style="border-collapse: collapse;font-weight: 400 !important;
                    font-size: 14px;" width="100%" height="80%">
                        <tr>
                            <td class="nameTd">建筑类型：</td>
                            <td class="priceTd">
                                {{efmDetail.device_place}}
                            </td>
                            <td class="nameTd">设 备 数：</td>
                            <td class="priceTd">
                                {{efmInfo.equipCount}}
                            </td>
                            <td align="left" class="nameTd">报 警 数：</td>
                            <td align="left" class="priceTd">
                                {{efmInfo.alarmCount}}
                            </td>
                        </tr>
                        <tr>
                            <td class="nameTd">预留电话：</td>
                            <td class="priceTd"></td>
                            <td align="left" class="nameTd">离 线 数：</td>
                            <td align="left" class="priceTd">{{efmInfo.offlineCount}}</td>
                            <td align="left" class="nameTd">故障数：</td>
                            <td align="left" class="priceTd">{{efmInfo.faultCount}}</td>
                        </tr>
                        <!-- <tr>
                            <td class="nameTd">位&nbsp;&nbsp;&nbsp;&nbsp;置：</td>
                            <td colspan="5" class="priceTd"></td>
                        </tr> -->
                    </table>
                </div>
            </div>
        </div>
        <div class="pageBox">
            <!-- <div class="myNavigation">
                <el-breadcrumb separator="/" style="display: inline-block">
                    <el-breadcrumb-item>
                        <span style="font-size: 16px;color:#303313;">项目管理</span>
                    </el-breadcrumb-item>
                    <el-breadcrumb-item>
                        <span style="font-size: 14px;color:#606266;">无线中控信息</span>
                    </el-breadcrumb-item>
                </el-breadcrumb>
            </div> -->
            
            <div style="height:100px; margin:16px 16px 0 16px;">

                <div style=" height:40px;">
                    <div style="">
                        <!-- <el-select v-model="equipmentTypeValue" @change="select(equipmentTypeValue)" placeholder="选择设备类型">
                            <el-option
                            v-for="item in equipmentType"
                            :key="item.id"
                            :label="item.name"
                            :value="item.id">
                            </el-option>
                        </el-select> -->
                        <el-radio-group v-model="addType" @change="select" style="padding-left: 16px">
                            <!-- <el-radio-button v-for="(item, index) in equipmentType" :key="index" :label="index">{{item.name}}</el-radio-button> -->
                            <el-radio-button label="0">电弧探测器</el-radio-button>
                            <el-radio-button label="1">组合探测器</el-radio-button>

                        </el-radio-group>
                    </div>
                </div>

                <!--条件输入框-->
                <div style=" float: left; height:40px;margin-left: 16px">
                    <!--编号输入框-->
                    <div style="width:200px;">
                        <el-input v-model="inputBoxValue" placeholder="请输入关键字"></el-input>
                    </div>
                </div>

                <div style="float: left; height:40px; width:56px;margin-left: 10px">
                    <el-button class="v-1-0-0-Button" @click="common.page = 1;getMqttList()">查询</el-button>
                </div>

            </div>

            <div style="height:calc(100% - 176px);margin: 0 16px auto 16px;
            border: 1px solid #F0F2F5;overflow: auto">
                <div class="newTable" v-show="addType == 1" v-for="(item,index) in mqttDeviceData1" :key="index">
                    <span class="column column1" v-if="index < 9">0{{index + 1}}</span>
                    <span class="column column1" v-else>{{index + 1}}</span>
                    <span class="column column2">
                        <div>
                            <span>编号：</span>
                            <span>{{parseInt(item.device_code,16)}}-{{parseInt(item.lineCode) + 1}}-{{item.addr}}</span>
                        </div>
                        <div class="addressMsg">
                            <span>地址：</span>
                            <span>{{item.message}}</span>
                        </div>
                    </span>
                    <span class="column column3">
                        <div>温度</div>
                        <div class="column3Div">
                            <span :class="item.tem0 > item.temLimit0 ? 'temRed': ''">{{item.tem0}}</span>/<span :class="item.tem1 > item.temLimit1 ? 'temRed': ''">{{item.tem1}}</span>/<span :class="item.tem2 > item.temLimit2 ? 'temRed': ''">{{item.tem2}}</span>/<span :class="item.tem3 > item.temLimit3 ? 'temRed': ''">{{item.tem3}}</span>/<span :class="item.tem4 > item.temLimit4 ? 'temRed': ''">{{item.tem4}}</span>
                        </div>
                    </span>
                    <span class="column column4">
                        <div>电压</div>
                        <div class="column4Div">
                            <span>{{item.aVoltage}}</span>
                            <span>{{item.bVoltage}}</span>
                            <span>{{item.cVoltage}}</span>
                        </div>
                    </span>
                    <span class="column column5">
                        <div>电流</div>
                        <div class="column4Div">
                            <span>{{item.aCurrent}}</span>
                            <span>{{item.bCurrent}}</span>
                            <span>{{item.cCurrent}}</span>
                        </div>
                    </span>
                    <span class="column column6">
                        <div>漏电</div>
                        <div class="column4Div"><span>{{item.leakageCurrent}}</span></div>
                    </span>
                    <span class="column column7">
                        <div>状态</div>
                        <div>
                            <span v-if="item.is_alarm == -1"><img src="../../../assets/img/normal.png" alt="">正常</span>
                            <span v-if="item.is_alarm == 0"><img src="../../../assets/img/fault.png" alt="">报警</span>
                            <span v-if="item.is_alarm == 1"><img src="../../../assets/img/fault.png" alt="">故障</span>
                            <span v-if="item.is_alarm == 2"><img src="../../../assets/img/offLine.png" alt="">离线</span>
                        </div>
                    </span>
                    <span class="column column8">
                        <div>
                            <el-button
                                @click.native.prevent="checkMqttTerminal(item)"
                                type="text"
                                size="medium">
                                <span style="color: #387EE8;">查看</span>
                            </el-button>
                            <el-button
                                @click.native.prevent="reboot(item)"
                                type="text"
                                size="medium">
                                <span style="color: #84cd61;">重启</span>
                            </el-button>
                            <el-button
                                @click.native.prevent="delMqttTerminal(item)"
                                type="text"
                                size="medium">
                                <span style="color: red;">删除</span>
                            </el-button>
                        </div>
                        <div>
                            <el-button
                            @click.native.prevent="clearVoice(item)"
                            type="primary">消音</el-button>
                            <el-button
                            @click.native.prevent="getReset(item)" 
                            type="primary">复位</el-button>
                        </div>
                    </span>
                </div>
                <div class="newTable" v-show="addType == -1 || addType == 0 || addType == 2 || addType == 3 || addType == undefined" v-for="(item,index) in mqttDeviceData" :key="item.id">
                    <span class="column column1" v-if="index < 9">0{{index + 1}}</span>
                    <span class="column column1" v-else>{{index + 1}}</span>
                    <span class="column column2">
                        <div>
                            <span>编号</span>
                        </div>
                        <div>
                            <span>{{parseInt(item.device_code,16)}}-{{parseInt(item.lineCode) + 1}}-{{item.addr}}</span>
                        </div>
                    </span>
                    <span class="column" style="width:15%">
                        <div>
                            <span>设备类型</span>
                        </div>
                        <div>
                            <span v-if="item.type == 0">电弧探测器</span>
                            <span v-if="item.type == 1">组合式探测器</span>
                        </div>
                    </span>
                    <span class="column"  style="width:24%">
                        <div>
                            <span>地址</span>
                        </div>
                        <div>
                            <span>{{item.message}}</span>
                        </div>
                    </span>
                    <span class="column column7"  style="width:10%">
                        <div>状态</div>
                        <div>
                            <span v-if="item.is_alarm == -1"><img src="../../../assets/img/normal.png" alt="">正常</span>
                            <span v-if="item.is_alarm == 0"><img src="../../../assets/img/fault.png" alt="">报警</span>
                            <span v-if="item.is_alarm == 1"><img src="../../../assets/img/fault.png" alt="">故障</span>
                            <span v-if="item.is_alarm == 2"><img src="../../../assets/img/offLine.png" alt="">离线</span>
                        </div>
                    </span>
                    <span class="column column8"  style="width:20%">
                        <div>
                            <el-button
                                @click.native.prevent="checkMqttTerminal(item)"
                                type="text"
                                size="medium">
                                <span style="color: #387EE8;">查看</span>
                            </el-button>
                            <el-button
                                @click.native.prevent="reboot(item)"
                                type="text"
                                size="medium">
                                <span style="color: #84cd61;">重启</span>
                            </el-button>
                            <el-button
                                @click.native.prevent="delMqttTerminal(item)"
                                type="text"
                                size="medium">
                                <span style="color: red;">删除</span>
                            </el-button>
                        </div>
                        <div>
                            <el-button
                            @click.native.prevent="clearVoice(item)"
                            type="primary">消音</el-button>
                            <el-button
                            @click.native.prevent="getReset(item)" 
                            type="primary">复位</el-button>
                        </div>
                    </span>
                </div>
                <div class="noData" v-show="mqttDeviceData.length ==0 || (addType == 1 &&  mqttDeviceData1.length == 0)">暂无数据</div>
                
                
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
        <el-dialog :visible.sync="editNameBox" width="26%">
            <div style=" width:100%;text-align: center; font-size: 20px; margin: -10px auto auto auto;">
                修改中控名称
            </div>

            <div style=" width:55%; height:30%;margin: 20px auto auto auto;">
                <el-input v-model="efmNameInput" maxLength="6" placeholder="请输入中控名称"></el-input>
            </div>

            <div style="width:135px;margin: 20px auto auto auto; ">
                <el-button class="v-1-0-0-Button-Minor" style="width: 56px;" @click="editNameBox = false">
                    取 消</el-button>
                <el-button class="v-1-0-0-Button" style="width: 56px;margin-left: 16px;" @click="editEfmName">
                    确 定</el-button>
            </div>

        </el-dialog>
    </div>
</template>
<script>
    import http from '../../../common/http';
    import bus from '../../../common/bus';
    export default {
        name: 'mqttDeviceList',
        data() {
            return {
                routeParam:{},
                efmInfo:{
                    isOnline: 1,
                    equipCount: 1,
                    equipAlarmCount: 1,
                    equipOffLineCount: 1,
                    equipFaultCount: 1
                },
                efmDetail:{
                    device_place: '类型'
                },
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
                equipmentTypeValue:null,
                // equipmentType: [
                //     {name: '不限', id: -1},
                //     {name: '电弧探测器', id: 0},
                //     {name: '组合式探测器', id: 1},
                //     {name: '测温式探测器', id: 2},
                //     {name: '剩余电流', id: 3},
                // ],
                equipmentType: 0,
                mqttDeviceData:[],
                mqttDeviceData1:[],
                delMqttEquipId:0,
                inputBoxValue:"",
                multipleSelection: [],
                addType: 0,
                centerDialogVisible: false,//修改中控名对话框
                editNameBox:false,
                efmNameInput: ''
            }
        },
        created: function () {
            this.routeParam = this.$route.params;
            this.addType = this.routeParam.addType==null?0:this.routeParam.addType;
            this.equipmentTypeValue = this.routeParam.equipmentTypeValue >-1?this.routeParam.equipmentTypeValue:-1;
            this.inputBoxValue = this.routeParam.inputBoxValue?this.routeParam.inputBoxValue:'';
            this.common={
                total:0,            //总数据量
                pageSize:20,        //每页条数
                page:this.routeParam.page?this.routeParam.page:1,              //页码
                isPage:1,            //是否分页
                allSizes:[10,20,30], //每页条数筛选
                checkboxArr:[]       //多选框数组
            };
            this.actionList = JSON.parse(sessionStorage.getItem('action'));
            if(this.addType == 1){
                this.getMqttList(1);
            }
            this.getEfmInfo();
            this.getMqttList();
            bus.$on('mqttTerminalClick',(msg)=>{
                this.routeParam.siteId = msg.siteId;
                this.routeParam.efmName = msg.efmName;
                this.getMqttList();
                this.getEfmInfo();
            })
        },
        beforeDestroy () {
            bus.$off('mqttTerminalClick');
        },
        methods: {
            formatNumber: function (row) {
                return row.serialNumber = row.device_code+"-"+(parseInt(row.lineCode) + 1) + "-" + parseInt(row.addr);
            },
            formatType: function (row) {
                if(row.type == 0){
                    row.type = "电弧探测器";
                }
                else if(row.type == 1){
                    row.type = "组合式探测器";
                }
                else if(row.type == 2){
                    row.type = "测温式探测器";
                }
                else if(row.type == 3){
                    row.type = "剩余电流";
                }
                return row.type;
            },
            select(id){
                console.log(1,id)
                this.equipmentType = id;
                this.addType = id;
                // this.equipmentTypeValue = id;
                this.getMqttList(id)
            },
            getEfmInfo(){
                let url = this.tabelUrl;
                let data = {
                    siteCode: this.routeParam.siteCode,
                };
                this.apiPost(url, data).then((res) => {
                    this.efmInfo = res.data;
                    // this.efmDetail = this.efmInfo.getdeviceInfo;
                    
                });
            },
            checkEfm(){
                this.$router.push({
                    name:'centerControlInformation',
                    params:{
                        id:this.routeParam.id,
                        efmName:this.routeParam.efmName,
                        siteLabel:this.efmInfo.getdeviceInfo.siteName?this.efmInfo.getdeviceInfo.siteName:this.efmInfo.getdeviceInfo.site_code
                    }
                });
            },
            editEfmName(){
                let url = this.editEfmNameUrl;
                this.efmNameInput = '无线中控';
                let data = {
                    deviceId: this.routeParam.siteId,
                    niName:this.efmNameInput
                };
                this.apiPost(url, data).then((res) => {
                    if(res.status === 200){
                        this.editNameBox = false;
                        this.routeParam.efmName = this.efmNameInput;
                        this.$commonFn.showTip("修改成功", 1);
                        bus.$emit('initTree');
                    }
                    else{
                        this.$commonFn.showTip(res.message,3);
                    }
                });
            },
            getMqttList(id){
                let url = this.getMqttListUrl;
                let data = {
                    siteId: this.routeParam.siteId,
                    type:this.addType == null?-1:this.addType,
                    currentPage:this.common.page,
                    pageSize:this.common.pageSize,
                    inquire:this.inputBoxValue
                };
                console.log(this.routeParam)
                this.apiPost(url, data).then((res) => {
                    console.log(res)
                    if(id == 1 || this.addType == 1){
                        this.mqttDeviceData1 = res.data.pageList;
                    }
                    this.mqttDeviceData = res.data.pageList;
                    console.log(this.mqttDeviceData,this.addType)
                    this.common.total = res.data.total;
                });
            },
            handleSelectionChange(val) {
                this.common.checkboxArr = [];
                for (let i = 0;i < val.length;i++){
                    this.common.checkboxArr.push(val[i].id);
                }
            },
            handleSizeChange(val) {
                this.common.pageSize = val;
                this.common.page = 1;
                this.getMqttList();
            },
            handleCurrentChange(val) {
                this.common.page = val;
                this.getMqttList();
            },
            reboot(row){
                this.$confirm('确定重启该设备吗?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'device_restart',
                        qos: 1,
                        ID: "01" + row.id,
                        type: 3,
                        extraData:"00"
                    };
                    console.log(data)
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("重启指令发送成功", 1);
                    }, (err) => {

                    });
                }).catch(() => {

                });
            },
            checkMqttTerminal(row){
                row.site_id = this.routeParam.siteId;
                row.addType = this.addType;
                row.inputBoxValue = this.inputBoxValue;
                row.page = this.common.page;
                this.$router.push({
                    name:'mqttInformation',
                    params:row
                });
            },
            delMqttTerminal(row){
                this.delMqttEquipId = row.id;
                this.$commonFn.showConfirm(this.deleteSuccess);
            },
            clearVoice(row) {
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'alarm_silencing',
                        qos: 1,
                        ID: "01"+row.id,
                        type: 3,
                        extraData:""
                    };
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("消音发送成功", 1);
                    }, (err) => {
                        console.log(err);
                    });

                },

            deleteSuccess(){
                let url = this.deleteEquipUrl;
                let data = {
                    deviceIds:this.delMqttEquipId
                };
                console.log(data)
                this.apiPost(url, data).then((res) => {
                    if(res.status === 200){
                        this.$commonFn.showTip("删除成功",1);
                        if(this.common.total % this.common.pageSize == 1){
                            this.common.page -= 1
                        }
                        this.getMqttList();
                    }
                    else{
                        this.$commonFn.showTip(res.message,3);
                    }
                }, (err) => {

                });
            },
            formatState:function(row, column){
                let warnState;
                if(row.is_alarm == -1){
                    warnState="正常";
                }else if(row.is_alarm == 0){
                    warnState="报警";
                }else if(row.is_alarm == 1){
                    warnState="故障";
                }else if(row.is_alarm == 2){
                    warnState="离线";
                }
                return warnState;
            },
            test(row){

            },
            reset(row){

            },
            // 复位
            getReset(row){
                console.log(row)
                this.setParams("alarm_reset","01",row.id);
            },
            setParams(cmd,content,id){
                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message:cmd,
                    qos: 1,
                    ID: "01" + id,
                    type: 3,
                    extraData:content
                };
                this.apiPost(url, data).then((res) => {
                    console.log(res)
                    if (cmd == 'device_descriptor') {
                        this.editMsg = true;
                        this.msgBtn = "修改";
                    }
                    else if(cmd == 'longitude'){
                        this.editLng = true;
                        this.lngBtn = "修改";
                    }
                    else if(cmd == 'latitude'){
                        this.editLat = true;
                        this.latBtn = "修改";
                    }
                    else if(cmd == 'nb_phone_number'){
                        this.editNbNumber = true;
                        this.nbNumberBtn = "修改";
                    }
                    else if(cmd == 'nb_chip_serial_number'){
                        this.editIccid = true;
                        this.iccidBtn = "修改";
                    }
                    else if(cmd == '0temLimit'){
                        this.editTemLimit0 = true;
                        this.temLimit0Btn = "修改";
                    }
                    else if(cmd == '1temLimit'){
                        this.editTemLimit1 = true;
                        this.temLimit1Btn = "修改";
                    }
                    else if(cmd == '2temLimit'){
                        this.editTemLimit2 = true;
                        this.temLimit2Btn = "修改";
                    }
                    else if(cmd == '3temLimit'){
                        this.editTemLimit3 = true;
                        this.temLimit3Btn = "修改";
                    }
                    else if(cmd == '4temLimit'){
                        this.editTemLimit4 = true;
                        this.temLimit4Btn = "修改";
                    }
                    else if(cmd == 'leakageCurrentUpLimit'){
                        this.editLeakageCurrentUpLimit = true;
                        this.leakageCurrentUpLimitBtn = "修改";
                    }
                    else if(cmd == 'AVoltageDownLimit'){
                        this.editAVoltageDownLimit = true;
                        this.aVoltageDownLimitBtn = "修改";
                    }
                    else if(cmd == 'BVoltageDownLimit'){
                        this.editBVoltageDownLimit = true;
                        this.bVoltageDownLimitBtn = "修改";
                    }
                    else if(cmd == 'CVoltageDownLimit'){
                        this.editCVoltageDownLimit = true;
                        this.cVoltageDownLimitBtn = "修改";
                    }
                    else if(cmd == 'AVoltageUpLimit'){
                        this.editAVoltageUpLimit = true;
                        this.aVoltageUpLimitBtn = "修改";
                    }
                    else if(cmd == 'BVoltageUpLimit'){
                        this.editBVoltageUpLimit = true;
                        this.bVoltageUpLimitBtn = "修改";
                    }
                    else if(cmd == 'CVoltageUpLimit'){
                        this.editCVoltageUpLimit = true;
                        this.cVoltageUpLimitBtn = "修改";
                    }
                    else if(cmd == 'ACurrentUpLimit'){
                        this.editACurrentUpLimit = true;
                        this.aCurrentUpLimitBtn = "修改";
                    }
                    else if(cmd == 'BCurrentUpLimit'){
                        this.editBCurrentUpLimit = true;
                        this.bCurrentUpLimitBtn = "修改";
                    }
                    else if(cmd == 'CCurrentUpLimit'){
                        this.editCCurrentUpLimit = true;
                        this.cCurrentUpLimitBtn = "修改";
                    }
                    else if(cmd == 'arc_warning_sensitivity'){
                        this.editArcWarningSensitivity = true;
                        this.arcWarningSensitivityBtn = "修改";
                    }
                    else if(cmd == 'seartbeat_time'){
                        this.editSeartbeatTime = true;
                        this.seartbeatTimeBtn = "修改";
                    }
                    setTimeout(this.getEquipInfo,5000);
                });
            },
        },
        mixins: [http]
    }
</script>

<style scoped>
    .pageBox{
        height: calc(100% - (28% + 60px));
    }
    .newTable{
        padding: 20px;
        margin-bottom: 8px;
        background: #f6f7fb;
        display: flex;
        justify-content: space-between;
        border-radius: 5px;
    }
    .column{
        color: #606266;
        
    }
    .column>div{
        line-height: 36px;
        overflow: hidden;
    }
    .column1{
        width: 5%;
        line-height:36px;
        font-size:24px;
        color:#303133
    }
    .column2{
        width: 16%;
        color: #303133;
    }
    .column3{
        width: 18%;
    }
    .column3Div{
        font-size: 18px;
    }
    .column4{
        width: 12%;
    }
    .column4Div span{
        padding: 0 3px;
        border-radius: 10px;
        text-align: center;
        border: 1px solid #d8dce6;
        font-size: 18px;
    }
    .column5{
        width: 12%;
    }
    .column6{
        width: 10%;
    }
    .column7{
        width: 8%;
    }
    .column7 img{
        padding-right: 2px;
    }
    .column8{
        width: 16%;
        text-align: right;
    }
    .noData{
        color: #909399;
        /* line-height: 60px; */
        /* text-align: center; */
        font-size: 14px;
        position: absolute;
        left: 56%;
        top: 50%;
        transform: translate(-50%,-50%);
        -webkit-transform: translate(-50%,-50%);
        -ms-transform: translate(-50%,-50%);
    }
    .temRed{
        color: #f57576;
    }
    .nameTd {
        letter-spacing: 3px;
        text-align: center;
        padding: 8px;
        border-style: solid;
        border-color: #d8dce6;
        background-color: #F6F7FB;
        width: 130px;
    }
    .priceTd {
        padding: 8px;
        height: 20px;
        border-style: solid;
        border-color: #d8dce6;
        background-color: white;
        width: 160px;
        text-indent: 10px;
    }
    .addressMsg{
        display: flex;
        justify-content: space-between;
    }
    .addressMsg span:first-child{
        width: 25%;
    }
    .addressMsg span:last-child{
        width: 75%;
        text-align: left;
    }
</style>
