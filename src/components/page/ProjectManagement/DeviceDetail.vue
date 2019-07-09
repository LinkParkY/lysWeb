<template id="deviceDetail">
    <div style="height: 100%;">
        <!--数据页面-->
        <div class="efm-box"
             style="overflow-y:auto;height: 100%; width:calc(100% - 200px); background-color: #F0F2F5; float: left;">
            <div style="height:28%;margin: 20px 24px;background-color: #fff">
                <div style="height:100%;margin: 16px 20px;background-color: #fff">
                    <div style="height: 25px;width:100%;padding-top: 12px;">
                        <div style="height: 25px;width:100%;padding-top: 0;">
                            <el-breadcrumb separator="/" style="display: inline-block">
                                <el-breadcrumb-item>
                                    <span style="font-size: 16px;color:#303313;">项目管理</span>
                                </el-breadcrumb-item>
                                <el-breadcrumb-item>
                                    <span style="font-size: 14px;color:#606266;">有线中控信息</span>
                                </el-breadcrumb-item>
                            </el-breadcrumb>
                        </div>
                    </div>
                    <div style="width: 100%;height: 16%; margin-top: 1%; margin-left: -2px;">
                        <div style="margin-top: 0; float: left;">
                            <i class="okay myIcon-xiangmu" style="font-size: 23px;color: orange;"></i>
                            <span style="font-size: 20px;font-weight: 500;">项目：</span>
                            <span>
                                <i v-show="efmInfo.isOnline == -1" class="el-icon-success"
                                   style="color: limegreen;position: relative;top: -1px;"></i>
                                <i v-show="efmInfo.isOnline == 2" class="el-icon-warning"
                                   style="color: lightgray;position: relative;top: -1px"></i>
                                <i v-show="efmInfo.isOnline == 1" class="el-icon-warning"
                                   style="color: red;position: relative;top: -1px"></i>
                            </span>&nbsp;
                            <span style="font-size: 20px;font-weight: 500;">{{routeParam.efmName}}</span>
                            <span style="font-size: 10px;font-weight: 300;color: #2d8cf0; cursor: pointer;"
                                  @click="centerDialogVisible = true">
                        <i class="okay myIcon-xiugai" style="font-size: 15px"
                           @click="editNameBox = true;efmNameInput = routeParam.efmName"></i>
                        </span>
                        </div>
                        <div style="float: right;position: relative;left: 2px;">
                            <el-button class="v-1-0-0-Button" @click="checkEfm">查看</el-button>
                        </div>

                    </div>

                    <div style=" height: 52%; margin: 10px 0; ">
                        <table class="gridtable" style="border-collapse: collapse;font-weight: 400 !important;
                        font-size: 14px;" width="100%" height="80%">
                            <tr>
                                <td class="nameTd">建筑类型：</td>
                                <td class="priceTd">
                                    {{efmDetail.buildName}}
                                </td>
                                <td class="nameTd">设 备 数：</td>
                                <td class="priceTd">
                                    {{efmInfo.equipCount}}
                                </td>
                                <td align="left" class="nameTd">报 警 数：</td>
                                <td align="left" class="priceTd">
                                    {{efmInfo.equipAlarmCount}}
                                </td>
                            </tr>
                            <tr>
                                <td class="nameTd">预留电话：</td>
                                <td align="left" class="priceTd">{{efmDetail.message_phone}}</td>
                                <td align="left" class="nameTd">离 线 数：</td>
                                <td align="left" class="priceTd">{{efmInfo.equipOffLineCount}}</td>
                                <td align="left" class="nameTd">故 障 数：</td>
                                <td align="left" class="priceTd">{{efmInfo.equipFaultCount}}</td>
                            </tr>
                            <!--                            <tr>-->
                            <!--                                <td class="nameTd">位&nbsp;&nbsp;&nbsp;&nbsp;置：</td>-->
                            <!--                                <td colspan="5" class="priceTd"></td>-->
                            <!--                            </tr>-->
                        </table>
                    </div>
                </div>
            </div>

            <div style="height:calc(100% - (28% + 60px)) ;margin: 20px 24px;background-color: #fff;min-height:500px;">
                <div style="padding-top: 12px;height: calc(100% - 10px)">
                    <div style="width:100%;background:#fff; height: 100%;overflow: hidden;">
                        <el-radio-group v-model="equipType" @change="changeEquipType" style="padding-left: 16px">
                            <el-radio-button label="0">故障电弧</el-radio-button>
                            <el-radio-button label="1">组合探测器</el-radio-button>
                        </el-radio-group>
                        <div style="margin-top: 10px;padding-left: 16px;padding-right: 16px">
                            <el-input v-model="equipInput" placeholder="请输入" style="width: 200px;"></el-input>
                            <el-button class="v-1-0-0-Button" style="margin-left: 5px;" @click="getArcEquip">确定
                            </el-button>
                            <el-button class="v-1-0-1-Button"
                                       v-if="actionList.indexOf('deleteDevice')>-1 && equipType == 0 "
                                       icon="el-icon-delete" @click="batchDelete" style="float: right">批量删除
                            </el-button>
                            <el-button class="v-1-0-0-Button" v-if="equipType == 0" @click="alertClear"
                                       style="float: right">广播消音
                            </el-button>
                        </div>

                        <div style="height:calc(100% - 146px);margin: 15px 16px 0 16px;overflow: auto">
                            <!--                            <el-table v-show="equipType == 0" ref="multipleTable" height="100%" :data="equipList"-->
                            <!--                                      tooltip-effect="dark" style="width: 100%" @selection-change="handleSelectionChange" :header-cell-style="tableStyle">-->

                            <!--                                <el-table-column type="selection" width="48">-->
                            <!--                                </el-table-column>-->

                            <!--                                <el-table-column type="index" width="56" label="#">-->
                            <!--                                </el-table-column>-->

                            <!--                                <el-table-column prop="serialNumber" :formatter="formatNumber" label="编号">-->
                            <!--                                </el-table-column>-->

                            <!--                                <el-table-column prop="message" label="信息" show-overflow-tooltip>-->
                            <!--                                </el-table-column>-->

                            <!--                                <el-table-column prop="tempration" label="温度" v-if="equipType == '1'" :key="Math.random()">-->
                            <!--                                </el-table-column>-->

                            <!--                                <el-table-column prop="broken_num" label="电压/电流" v-if="equipType == '1'" :key="Math.random()">-->
                            <!--                                </el-table-column>-->

                            <!--                                <el-table-column prop="is_alarm" :formatter="formatState" label="状态"></el-table-column>-->
                            <!--                                <el-table-column fixed="right" label="操作">-->
                            <!--                                    <template slot-scope="scope">-->
                            <!--                                        <el-button-->
                            <!--                                            @click.native.prevent="checkTerminal(scope.row)"-->
                            <!--                                            type="primary"-->
                            <!--                                            size="small">-->
                            <!--                                            <span>查看</span>-->
                            <!--                                        </el-button>-->
                            <!--                                        &lt;!&ndash; <el-button style="color: #84cd61;"-->
                            <!--                                                   v-if="actionList.indexOf('deleteDevice')>-1"-->
                            <!--                                            @click.native.prevent="delTerminal(scope.row)"-->
                            <!--                                            type="text"-->
                            <!--                                            size="small">-->
                            <!--                                            <span style="color: #387EE8;">重启</span>-->
                            <!--                                        </el-button> &ndash;&gt;-->
                            <!--                                        <el-button style="color: red;"-->
                            <!--                                                   v-if="actionList.indexOf('deleteDevice')>-1"-->
                            <!--                                            @click.native.prevent="delTerminal(scope.row)"-->
                            <!--                                            type="primary"-->
                            <!--                                            size="small">-->
                            <!--                                            <span>删除</span>-->
                            <!--                                        </el-button>-->
                            <!--                                    </template>-->
                            <!--                                </el-table-column>-->
                            <!--                            </el-table>-->
                            <div class="newTable"    v-show="equipType == 0" v-for="(item,index) in equipList" :key="index">


                                <span class="column column1" v-if="index < 9">0{{index + 1}}</span>
                                <span class="column column1" v-else>{{index + 1}}</span>
                                <span class="column column2">
                                    <div>
                                         <span>编号</span>
                                    </div>
                                    <div>
                                         <span>{{item.deviceCode}}-{{parseInt(item.lineCode) + 1}}-{{item.addr}}</span>
                                    </div>
                                </span>
                                <span class="column" style="width:24%">
                                    <div>
                                        <span>地址</span>
                                    </div>
                                    <div>
                                        <span>{{item.message}}</span>
                                    </div>
                                </span>
                                <span class="column column7" style="width:10%">
                                    <div>状态</div>
                                    <div>
                                        <span v-if="item.is_alarm == -1"><img src="../../../assets/img/normal.png" alt="">正常</span>
                                        <span v-if="item.is_alarm == 0"><img src="../../../assets/img/fault.png" alt="">报警</span>
                                        <span v-if="item.is_alarm == 1"><img src="../../../assets/img/fault.png" alt="">故障</span>
                                        <span v-if="item.is_alarm == 2"><img src="../../../assets/img/offLine.png" alt="">离线</span>
                                    </div>
                                </span>
                                <span class="column column8" style="width:20%">

                                    <div>
                                        <el-button
                                            @click.native.prevent="checkTerminal(item)"
                                            type="primary">查看</el-button>
                                        <el-button
                                            @click.native.prevent="delTerminal(item)"
                                            type="primary">删除</el-button>
                                    </div>
                                 </span>
                            </div>

                            <div class="newTable" v-show="equipType == 1" v-for="(item,index) in equipList1"  :key="index">

                                <span class="column column1" v-if="index < 9">0{{index + 1}}</span>
                                <span class="column column1" v-else>{{index + 1}}</span>
                                <span class="column column2">
                                    <div>
                                        <span>编号：</span>
                                        <span>{{item.deviceCode}}-{{parseInt(item.lineCode) + 1}}-{{item.addr}}</span>
                                    </div>
                                    <div>
                                        <span>地址：</span>
                                        <span>{{item.message}}</span>
                                    </div>
                                </span>
                                <span class="column column3">
                                    <div>温度</div>
                                    <div class="column3Div">
                                        <span
                                            :class="item.tem0 > item.temLimit0 ? 'temRed': ''">{{item.tem0}}</span>/<span
                                        :class="item.tem1 > item.temLimit1 ? 'temRed': ''">{{item.tem1}}</span>/<span
                                        :class="item.tem2 > item.temLimit2 ? 'temRed': ''">{{item.tem2}}</span>/<span
                                        :class="item.tem3 > item.temLimit3 ? 'temRed': ''">{{item.tem3}}</span>/<span
                                        :class="item.tem4 > item.temLimit4 ? 'temRed': ''">{{item.tem4}}</span>
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
                                        <span v-if="item.is_alarm == -1"><img src="../../../assets/img/normal.png"
                                                                              alt="">正常</span>
                                        <span v-if="item.is_alarm == 0"><img src="../../../assets/img/fault.png" alt="">报警</span>
                                        <span v-if="item.is_alarm == 1"><img src="../../../assets/img/fault.png" alt="">故障</span>
                                        <span v-if="item.is_alarm == 2"><img src="../../../assets/img/offLine.png"
                                                                             alt="">离线</span>
                                    </div>
                                </span>
                                <span class="column column8">
                                    <div>
                                        <el-button
                                            @click.native.prevent="checkTerminal(item)"
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
                                            @click.native.prevent="delTerminal(item)"
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
                            <div class="noData" v-if="equipList.length == 0">暂无数据</div>

                        </div>
                        <div style="float:right;margin-right: 13px;">
                            <el-pagination
                                @size-change="handleSizeChange"
                                @current-change="handleCurrentChange"
                                :page-size="common.pageSize"
                                :page-sizes="common.allSizes"
                                layout="sizes, total, prev, pager, next"
                                :total="common.total">
                            </el-pagination>
                        </div>
                    </div>
                </div>

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
                    取 消
                </el-button>
                <el-button class="v-1-0-0-Button" style="width: 56px;margin-left: 16px;" @click="editEfmName">
                    确 定
                </el-button>
            </div>

        </el-dialog>
    </div>

</template>
<script>
    import http from '../../../common/http';
    import bus from '../../../common/bus';

    export default {
        name: 'deviceDetail',
        data() {
            return {
                routeParam: {},
                efmInfo: {},
                efmDetail: {},
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
                equipInput: "",
                equipType: 0,
                editNameBox: false,
                efmNameInput: "",
                delEquipId: 0,          //删除的终端id
                centerDialogVisible: false,//修改中控名对话框
                is_site: true,
                is_device: false,

                editSite: {
                    site_code: "",
                    site_name: "",
                    site_place: "",
                    site_build_id: "",
                    site_id: 0
                },
                equipList: [], //终端列表
                equipList1: [], //终端列表1
                multipleSelection: [],
            }
        },
        created: function () {
            this.routeParam = this.$route.params;
            console.log(this.routeParam)
            this.actionList = JSON.parse(sessionStorage.getItem('action'));
            this.getEfmInfo();
            this.getArcEquip();
            bus.$on('efmClickEfmClick', (msg) => {
                this.routeParam.id = msg.id;
                this.routeParam.efmName = msg.efmName;
                this.getEfmInfo();
                this.getArcEquip();
            })
        },
        beforeDestroy() {
            bus.$off('efmClickEfmClick');
        },
        methods: {
            getEfmInfo() {
                let url = this.getEfmInfoUrl;
                let data = {
                    deviceId: this.routeParam.id,
                };
                this.apiPost(url, data).then((res) => {
                    this.efmInfo = res.data;
                    this.efmDetail = this.efmInfo.getdeviceInfo;
                    console.log(res);
                });
            },
            getArcEquip() {
                let _loading = this.$commonFn.showLoading(2, '.efm-box');
                let url = this.getEquipListUrl;
                let data = {
                    deviceId: this.routeParam.id,
                    pagesize: this.common.pageSize,
                    currentPage: this.common.page,
                    type: this.equipType,
                    inquire: this.equipInput
                };
                if (this.equipInput.indexOf("-") > 0) {
                    data.parameterType = 1;
                    let arr = this.equipInput.split('-');
                    let arr1 = parseInt(arr[0]) - 1;
                    data.inquire = arr1 + '-' + arr[1];
                } else {
                    data.parameterType = 0;
                }
                this.apiPost(url, data).then((res) => {
                    _loading.close();
                    if (res.status === 200) {
                        console.log(res.data)
                        //为了解决切换按钮一闪而过的数据展示
                        if (data.type == 1) {
                            this.equipList1 = res.data.equipList;
                        }
                        this.equipList = res.data.equipList;
                        console.log(data.type, this.equipList)
                        this.common.total = res.data.equipListCount;
                        for (let i in this.equipList) {
                            //临时数据
                            this.equipList[i].tempration = this.equipList[i].tempration + "/" + this.equipList[i].tempration + "/" + this.equipList[i].tempration
                            this.equipList[i].broken_num = "220V/1.0A";
                        }
                    } else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {
                    _loading.close();
                });
            },
            // 消音
            clearVoice(row) {
                console.log(row)
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    message: 'alarm_silencing',
                    qos: 1,
                    ID: "01" + row.id,
                    type: 3,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    this.$commonFn.showTip("消音发送成功", 1);
                }, (err) => {
                    console.log(err);
                });

            },
            // 重启
            reboot(row) {
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
                        extraData: "00"
                    };
                    console.log(data)
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("重启指令发送成功", 1);
                    }, (err) => {

                    });
                }).catch(() => {

                });
            },
            // 复位
            getReset(row) {
                this.setParams("alarm_reset", "01", row.id);
            },
            setParams(cmd, content, id) {
                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message: cmd,
                    qos: 1,
                    ID: "01" + id,
                    type: 3,
                    extraData: content
                };
                console.log(data)
                this.apiPost(url, data).then((res) => {
                    if (cmd == 'device_descriptor') {
                        this.editMsg = true;
                        this.msgBtn = "修改";
                    } else if (cmd == 'longitude') {
                        this.editLng = true;
                        this.lngBtn = "修改";
                    } else if (cmd == 'latitude') {
                        this.editLat = true;
                        this.latBtn = "修改";
                    } else if (cmd == 'nb_phone_number') {
                        this.editNbNumber = true;
                        this.nbNumberBtn = "修改";
                    } else if (cmd == 'nb_chip_serial_number') {
                        this.editIccid = true;
                        this.iccidBtn = "修改";
                    } else if (cmd == '0temLimit') {
                        this.editTemLimit0 = true;
                        this.temLimit0Btn = "修改";
                    } else if (cmd == '1temLimit') {
                        this.editTemLimit1 = true;
                        this.temLimit1Btn = "修改";
                    } else if (cmd == '2temLimit') {
                        this.editTemLimit2 = true;
                        this.temLimit2Btn = "修改";
                    } else if (cmd == '3temLimit') {
                        this.editTemLimit3 = true;
                        this.temLimit3Btn = "修改";
                    } else if (cmd == '4temLimit') {
                        this.editTemLimit4 = true;
                        this.temLimit4Btn = "修改";
                    } else if (cmd == 'leakageCurrentUpLimit') {
                        this.editLeakageCurrentUpLimit = true;
                        this.leakageCurrentUpLimitBtn = "修改";
                    } else if (cmd == 'AVoltageDownLimit') {
                        this.editAVoltageDownLimit = true;
                        this.aVoltageDownLimitBtn = "修改";
                    } else if (cmd == 'BVoltageDownLimit') {
                        this.editBVoltageDownLimit = true;
                        this.bVoltageDownLimitBtn = "修改";
                    } else if (cmd == 'CVoltageDownLimit') {
                        this.editCVoltageDownLimit = true;
                        this.cVoltageDownLimitBtn = "修改";
                    } else if (cmd == 'AVoltageUpLimit') {
                        this.editAVoltageUpLimit = true;
                        this.aVoltageUpLimitBtn = "修改";
                    } else if (cmd == 'BVoltageUpLimit') {
                        this.editBVoltageUpLimit = true;
                        this.bVoltageUpLimitBtn = "修改";
                    } else if (cmd == 'CVoltageUpLimit') {
                        this.editCVoltageUpLimit = true;
                        this.cVoltageUpLimitBtn = "修改";
                    } else if (cmd == 'ACurrentUpLimit') {
                        this.editACurrentUpLimit = true;
                        this.aCurrentUpLimitBtn = "修改";
                    } else if (cmd == 'BCurrentUpLimit') {
                        this.editBCurrentUpLimit = true;
                        this.bCurrentUpLimitBtn = "修改";
                    } else if (cmd == 'CCurrentUpLimit') {
                        this.editCCurrentUpLimit = true;
                        this.cCurrentUpLimitBtn = "修改";
                    } else if (cmd == 'arc_warning_sensitivity') {
                        this.editArcWarningSensitivity = true;
                        this.arcWarningSensitivityBtn = "修改";
                    } else if (cmd == 'seartbeat_time') {
                        this.editSeartbeatTime = true;
                        this.seartbeatTimeBtn = "修改";
                    }
                    setTimeout(this.getEquipInfo, 5000);
                });
            },
            //修改中控名称
            editEfmName() {
                let url = this.editEfmNameUrl;
                let data = {
                    deviceId: this.routeParam.id,
                    niName: this.efmNameInput
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.editNameBox = false;
                        this.routeParam.efmName = this.efmNameInput;
                        this.$commonFn.showTip("修改成功", 1);
                        bus.$emit('initTree');
                    } else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                });
                // let url = this.setDeviceParamsUrl;
                // let data = {
                //     siteCode: this.routeParam.id.substring(0,8),
                //     deviceCode:this.routeParam.id.substring(8,10),
                //     protocolName:"udp_set_specificator",
                //     extraData:this.efmNameInput
                // };
                // this.apiPost(url, data).then((res) => {
                //     if(res.status === 200){
                //         localStorage.setItem('cmd',"udp_set_specificator");
                //         localStorage.setItem('efmId',this.routeParam.id.substring(0,8) + this.routeParam.id.substring(8,10));
                //         let that = this;
                //         setTimeout(function () {
                //             localStorage.removeItem('cmd');
                //             localStorage.removeItem('efmId');
                //             //that.$commonFn.showTip("消息超时", 2);
                //         },5000)
                //     }
                //     else{
                //         this.$commonFn.showTip(res.message,3);
                //     }
                // });
            },
            alertClear() {
                let url = this.sendAdminSocketUrl;
                let data = {
                    protocolName: 'udp_clear_broadcast_voice',
                    siteCode: this.efmDetail.site_code,
                    deviceCode: this.efmDetail.device_code,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        localStorage.setItem('cmd', "udp_clear_broadcast_voice");
                        localStorage.setItem('efmId', this.efmDetail.site_code + this.efmDetail.device_code);
                        let that = this;
                        this.$constant.setCmdTime = setTimeout(function () {
                            that.apiPost(url, data).then((res) => {
                                if (res.status === 200) {
                                    let self = that;
                                    that.$constant.setCmdTime = setTimeout(function () {
                                        localStorage.removeItem('cmd');
                                        localStorage.removeItem('efmId');
                                        that.$commonFn.showTip("消息超时", 2);
                                    }, 4000)
                                } else {
                                    that.$commonFn.showTip(res.message, 3);
                                }
                            }, (err) => {
                                console.log(err);
                            });
                        }, 4000)
                    } else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {
                    console.log(err);
                });
            },
            checkEfm() {
                this.$router.push({
                    name: 'centerControlInformation',
                    params: {
                        id: this.routeParam.id,
                        efmName: this.routeParam.efmName,
                        siteLabel: this.efmInfo.getdeviceInfo.siteName ? this.efmInfo.getdeviceInfo.siteName : this.efmInfo.getdeviceInfo.site_code
                    }
                });
            },
            checkTerminal(data) {
                console.log(data);
                this.$router.push({
                    name: 'terminalInformation',
                    params: {
                        equipId: data.id,
                        efmId: this.routeParam.id,
                        efmName: this.routeParam.efmName,
                        siteLabel: this.efmInfo.getdeviceInfo.siteName ? this.efmInfo.getdeviceInfo.siteName : this.efmInfo.getdeviceInfo.site_code
                    }
                });
            },
            delTerminal(row) {
                console.log(row)
                this.delEquipId = row.id;
                this.$commonFn.showConfirm(this.deleteSuccess);
            },
            deleteSuccess() {
                let url = this.deleteEquipUrl;
                let data = {
                    deviceIds: this.delEquipId
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.$commonFn.showTip("删除成功", 1);
                        this.common.page = 1;
                        this.getArcEquip();
                    } else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {

                });
            },
            batchDelete() {
                if (this.common.checkboxArr.length != 0) {
                    let idStr = this.common.checkboxArr.join(",");
                    this.delEquipId = idStr;
                    this.$commonFn.showConfirm(this.deleteSuccess);
                } else {
                    this.$commonFn.showTip("请选择删除数据", 2);
                }
            },
            formatNumber: function (row, column) {
                return row.serialNumber = parseInt(row.line_code) + 1 + "-" + row.addr;
            },
            formatState: function (row, column) {
                let warnState;
                if (row.is_alarm == -1) {
                    warnState = "正常";
                } else if (row.is_alarm == 0) {
                    warnState = "报警";
                } else if (row.is_alarm == 1) {
                    warnState = "故障";
                } else if (row.is_alarm == 2) {
                    warnState = "离线";
                }
                return warnState;
            },

            //项目详情查看
            projectDetail(node, data) {
                console.log(data);
                this.is_site = true;
                this.is_device = false;
                this.editSite.site_code = data.siteCode;
                this.editSite.site_name = data.label;
                this.editSite.site_place = data.sitePlace;
                this.editSite.site_build_id = data.siteBuildId;
                this.editSite.site_id = data.siteId;
            },
            //切换终端类型
            changeEquipType(type) {
                console.log(type)
                this.equipType = type;
                this.getArcEquip();
            },
            handleSelectionChange(val) {
                console.log(val, 111)
                this.common.checkboxArr = [];
                for (let i = 0; i < val.length; i++) {
                    this.common.checkboxArr.push(val[i].id);
                }
            },
            handleSizeChange(val) {
                this.common.pageSize = val;
                this.common.page = 1;
                this.getArcEquip();
            },
            handleCurrentChange(val) {
                this.common.page = val;
                this.getArcEquip();
            },

            // handleScroll () {
            //     let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
            //     console.log(scrollTop)
            // },
        },
        // mounted () {
        //     window.addEventListener('scroll', this.handleScroll,true)
        // },
        mixins: [http]
    }
</script>

<style scoped>
    table.gridtable {
        padding: 8px;
        font-family: verdana, arial, sans-serif;
        font-size: 11px;
        color: #333333;
        border-width: 1px;
        /* border-color: #666666;*/
        border-color: #d8dce6 !important;
        border-collapse: collapse;
    }

    table.gridtable th:nth-child(odd) {
        border-width: 1px;
        padding: 8px;
        height: 18px;
        border-style: solid;
        /*  border-color: #666666;*/
        /* border-color: rgb(154,200,228);*/
        border-color: #d8dce6;
        background-color: #F6F7FB;
        /* background-color: #dadee9;*/
        text-align: center;
    }

    table.gridtable th:nth-child(even) {
        border-width: 1px;
        height: 18px;
        padding: 0px;
        border-style: solid;
        /*  border-color: #666666;*/
        /* border-color: rgb(154,200,228);*/
        border-color: #d8dce6;
        background-color: white;
        /* background-color: #dadee9;*/
        text-align: left;
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

    .login-box {
        position: absolute;
        left: 50%;
        top: 60%;
        /*width:298px;*/
        /*height:288px;*/
        margin: -146px 0 0 -163px;
        padding: 30px;
        border-radius: 5px;
        background: rgba(5, 5, 38, 0.2);
        border: 1px solid rgba(5, 5, 38, 0.3);
    }

    .gridcontainer {
        display: grid;
        grid-template-columns: 10rem 2px auto;
        grid-template-rows: auto;
        background: #325AA2;
    }

    .el-table th {
        text-align: center;
    }

    .el-table tbody tr td:first-child {
        text-align: center;
    }

    .el-pagination {
        padding: 15px 5px;
    }

    .table-scroll-mod {
        position: relative;
        height: 65px;
        overflow: hidden;
    }


    .gridcontainer[data-v-cc10f246] {
        width: 0;
        background: rgb(16, 42, 93);
    }

    /* .deviceMenu {
        background: white;
        width: 200px;
        height: 100%;
        float: left;
        overflow-y: auto !important;
    } */

    .el-tree .el-tree-node:hover {
        background: red !important;
    }

    .custom-tree-node {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        padding-right: 8px;
    }

    .mySpan {
        cursor: pointer;
        color: cornflowerblue;
    }

    hr {
        background-color: #F0F2F5;
        height: 1px;
        border: none;
    }

    .v-1-0-0-Button {
        background-color: #387EE8;
        color: white;
    }

    .v-1-0-0-Button-Minor {
        background-color: #F0F8FF;
        color: #387EE8;
    }

    .el-tabs__content {
        height: calc(100% - 108px) !important;
    }

    .el-tabs {
        height: 100% !important;
    }

    .newTable {
        padding: 20px;
        margin-bottom: 8px;
        background: #f6f7fb;
        display: flex;
        justify-content: space-between;
        border-radius: 5px;
    }

    .column {
        color: #606266;

    }

    .column > div {
        line-height: 36px;
        overflow: hidden;
    }

    .column1 {
        width: 5%;
        line-height: 36px;
        font-size: 24px;
        color: #303133
    }

    .column2 {
        width: 16%;
        color: #303133;
    }

    .column3 {
        width: 18%;
    }

    .column3Div {
        font-size: 20px;
    }

    .column4 {
        width: 12%;
    }

    .column4Div span {
        padding: 0 3px;
        border-radius: 10px;
        text-align: center;
        border: 1px solid #d8dce6;
    }

    .column5 {
        width: 12%;
    }

    .column6 {
        width: 10%;
    }

    .column7 {
        width: 8%;
    }

    .column7 img {
        padding-right: 2px;
    }

    .column8 {
        width: 16%;
        text-align: right;
    }

    .noData {
        color: #666;
        line-height: 60px;
        text-align: center;
    }

    .temRed {
        color: #f57576;
    }
</style>
