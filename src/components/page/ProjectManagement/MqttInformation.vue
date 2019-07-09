<template>

    <div style="height: 100%;">
        <div style="height: 100%; width:calc(100% - 200px); background-color: #F0F2F5; float: left;">
            <div class="pageBox">
                <div class="myNavigation">
                    <el-breadcrumb separator="/" style="display: inline-block">
                        <el-breadcrumb-item>
                            <span style="font-size: 16px;color:#303313;">项目管理</span>
                        </el-breadcrumb-item>
                        <el-breadcrumb-item>
                            <span style="color: #111111;font-size: 14px;color:#606266;">无线终端详情</span>
                        </el-breadcrumb-item>
                    </el-breadcrumb>
                </div>

                <div style="display: inline-block; height:40px;float: right;margin: 0 16px 6px 0;">
                    <!--<el-button class="v-1-0-0-Button" icon="el-icon-setting" @click="checkTerminalAlarm">告警设置</el-button>-->
                </div>
                <div v-if="this.isSetLimit==0">
                    <div style=" margin: 24px 16px 1% 16px;">
                        <table style="width: 100%; font-size: 14px;table-layout:fixed">
                            <tr>
                                <td width="145px" style="background-color: #F6F7FB;">
                                    项&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    目 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.siteLabel}}</td>
                                <td width="145px" style="background-color: #F6F7FB;">
                                    设&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    备 ：
                                </td>
                                <td class="pricrTd">无线终端</td>
                                <td width="145px" style="background-color: #F6F7FB;">

                                    编&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    号 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.serialNumber}}</td>
                            </tr>

                            <tr v-if="this.routeParam.type=='1' || this.routeParam.type=='剩余电流' ">
                                <td style="background-color: #F6F7FB;">
                                    类&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    型 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.type}}</td>

                                <td style="background-color: #F6F7FB;">
                                    型&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    号 ：
                                </td>
                                <td class="pricrTd">
                                    {{equipInfo.equipmentType}}
                                </td>
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    硬&nbsp;&nbsp;件&nbsp;&nbsp;版&nbsp;&nbsp;本 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.boardVersion}}</td>
                            </tr>
                            <tr v-if="this.routeParam.type=='0' ">
                                <td style="background-color: #F6F7FB;">
                                    类&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    型 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.type}}</td>

                                <td style="background-color: #F6F7FB;">
                                    型&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    号 ：
                                </td>
                                <td class="pricrTd">
                                    {{equipInfo.equipmentType='@'?"AFDD1000":equipInfo.equipmentType}}
                                </td>
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    软&nbsp;&nbsp;件&nbsp;&nbsp;版&nbsp;&nbsp;本 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.softVersion}}</td>
                            </tr>
                            <tr>
                                <!--<td style="background-color: #F6F7FB;letter-spacing: 0.2px;">-->
                                <!--发&nbsp;&nbsp;行&nbsp;&nbsp;时&nbsp;&nbsp;间 ：</td>-->
                                <!--<td class="pricrTd">{{equipInfo.releaseData}}</td>-->
                                <td style="background-color: #F6F7FB;letter-spacing: 3px;padding-left: 3px;">产品序列号：</td>
                                <td class="pricrTd " style="text-overflow: ellipsis;overflow: hidden;white-space: nowrap;">{{equipInfo.equipmentProductionSequenceNumber}}</td>
                                <td style="background-color: #F6F7FB;letter-spacing: 3px;padding-left: 3px;">I&nbsp;C&nbsp;C&nbsp;I&nbsp;D：</td>
                                <td class="pricrTd">{{equipInfo.nbChipSerialNumber}}</td>
                                <td style="background-color: #F6F7FB;">
                                    备&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    注 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.equip_place" :disabled="editPlace"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="changePlace"> {{placeBtn}}</span>
                                </td>
                            </tr>

                            <tr v-if="this.routeParam.type=='0'">
                                <!--<td style="background-color: #F6F7FB;">-->
                                <!--备&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-->
                                <!--注 ：</td>-->
                                <!--<td  class="pricrTd" style="padding: 0;text-indent: 0">-->
                                <!--<input type="text" v-model="equipInfo.equip_place" :disabled="editPlace" style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap" />-->
                                <!--<span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block" @click="changePlace"> {{placeBtn}}</span>-->
                                <!--</td>-->
                                <td style="background-color: #F6F7FB;letter-spacing: 1.5px;padding-left: 3px;">
                                    描&nbsp;&nbsp;&nbsp;述&nbsp;&nbsp;&nbsp;符 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.message" :disabled="editMsg"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setMsg(equipInfo.message)"> {{msgBtn}}</span>
                                </td>
                                <!--<td style="background-color: #F6F7FB;">单位电弧次数：</td>-->
                                <!--<td colspan="3" class="pricrTd">{{equipInfo.cnt}}</td>-->
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;" width="145px">
                                    温&nbsp;&nbsp;度&nbsp;&nbsp;上&nbsp;&nbsp;限 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.temLimit0" :disabled="editTemLimit0"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setTemLimit0(equipInfo.temLimit0)"> {{temLimit0Btn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;letter-spacing: 1.5px;padding-left: 3px;">
                                    灵&nbsp;&nbsp;&nbsp;敏&nbsp;&nbsp;&nbsp;度 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.arcWarningSensitivity"
                                           :disabled="editArcWarningSensitivity"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setArcWarningSensitivity(equipInfo.arcWarningSensitivity)"> {{arcWarningSensitivityBtn}}</span>

                                </td>
                            </tr>
                            <tr v-if="this.routeParam.type=='0'">
                                <!--<td style="background-color: #F6F7FB;">-->
                                <!--备&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-->
                                <!--注 ：</td>-->
                                <!--<td  class="pricrTd" style="padding: 0;text-indent: 0">-->
                                <!--<input type="text" v-model="equipInfo.equip_place" :disabled="editPlace" style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap" />-->
                                <!--<span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block" @click="changePlace"> {{placeBtn}}</span>-->
                                <!--</td>-->

                                <td style="background-color: #F6F7FB;" width="145px">
                                    经&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    度 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.lngBmap" :disabled="editLng"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setLng(equipInfo.lngBmap)"> {{lngBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;" width="145px">
                                    纬&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    度 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.latBmap" :disabled="editLat"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setLat(equipInfo.latBmap)"> {{latBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;" width="145px">
                                    告&nbsp;&nbsp;警&nbsp;&nbsp;使&nbsp;&nbsp;能 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <el-switch v-model="arcAlarmEnableValue" @change="setArcAlarmEnable">
                                    </el-switch>
                                </td>
                            </tr>
                            <tr v-if="this.routeParam.type=='1' || this.routeParam.type=='剩余电流' ">
                                <!--<td style="background-color: #F6F7FB;">-->
                                <!--备&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-->
                                <!--注 ：</td>-->
                                <!--<td  class="pricrTd" style="padding: 0;text-indent: 0">-->
                                <!--<input type="text" v-model="equipInfo.equip_place" :disabled="editPlace" style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap" />-->
                                <!--<span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block" @click="changePlace"> {{placeBtn}}</span>-->
                                <!--</td>-->
                                <td style="background-color: #F6F7FB;letter-spacing: 1.5px;padding-left: 3px;">
                                    描&nbsp;&nbsp;&nbsp;述&nbsp;&nbsp;&nbsp;符 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.message" :disabled="editMsg"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setMsg(equipInfo.message)"> {{msgBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;" width="145px">
                                    经&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    度 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.lngBmap" :disabled="editLng"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setLng(equipInfo.lngBmap)"> {{lngBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;" width="145px">
                                    纬&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                                    度 ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.latBmap" :disabled="editLat"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setLat(equipInfo.latBmap)"> {{latBtn}}</span>
                                </td>
                            </tr>


                            <tr>
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    服务器IP地址：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.serverIpAddress" :disabled="editIp"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setIp(equipInfo.serverIpAddress)"> {{ipBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;padding-left: 3px;">服务器端口号：</td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.serverPortNumber" :disabled="editPort"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setPort(equipInfo.serverPortNumber)"> {{portBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;">蜂鸣器使能：</td>
                                <td class="pricrTd">
                                    <el-switch v-model="buzzerEnableValue" @change="setBuzzerEnable">
                                    </el-switch>
                                </td>
                            </tr>
                            <tr v-if="this.routeParam.type=='1' || this.routeParam.type=='剩余电流' ">
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    软&nbsp;&nbsp;件&nbsp;&nbsp;版&nbsp;&nbsp;本 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.softVersion}}</td>
                                <td style="background-color: #F6F7FB;padding-left: 3px;">心&nbsp;&nbsp;跳&nbsp;&nbsp;时&nbsp;&nbsp;间
                                    ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.seartbeatTime" :disabled="editSeartbeatTime"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setSeartbeatTime(equipInfo.seartbeatTime)"> {{seartbeatTimeBtn}}</span>
                                </td>
                                <td width="145px" style="background-color: #F6F7FB;">域名地址：</td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.domainNameAddress" :disabled="editDomainNameAddress"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setDomainNameAddress(equipInfo.domainNameAddress)"> {{domainNameAddressBtn}}</span>
                                </td>

                            </tr>
                            <tr v-if="this.routeParam.type=='0'">
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    硬&nbsp;&nbsp;件&nbsp;&nbsp;版&nbsp;&nbsp;本 ：
                                </td>
                                <td class="pricrTd">{{equipInfo.boardVersion}}</td>
                                <td style="background-color: #F6F7FB;padding-left: 3px;">心&nbsp;&nbsp;跳&nbsp;&nbsp;时&nbsp;&nbsp;间
                                    ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.seartbeatTime" :disabled="editSeartbeatTime"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setSeartbeatTime(equipInfo.seartbeatTime)"> {{seartbeatTimeBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;padding-left: 3px;">测&nbsp;&nbsp;试&nbsp;&nbsp;&&nbsp;&nbsp;复&nbsp;&nbsp;位
                                    ：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    &nbsp;&nbsp;
                                    <!--<div style="display: inline-block"><el-button class="v-1-0-0-Button-Minor" @click="getTest">测试</el-button></div>-->
                                    <div style="display: inline-block">
                                        <el-button class="v-1-0-0-Button-Minor" @click="getReset">复位</el-button>
                                    </div>
                                </td>

                            </tr>
                            <tr>
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    备用服务器IP地址：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.standbyServerIpAddress"
                                           :disabled="editStandbyServerIpAddress"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setStandbyServerIpAddress(equipInfo.standbyServerIpAddress)"> {{standbyServerIpAddressBtn}}</span>
                                </td>
                                <td style="background-color: #F6F7FB;letter-spacing: 0.2px;">
                                    备用服务器端口号：
                                </td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.standbyServerPortNumber"
                                           :disabled="editStandbyServerPortNumber"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setStandbyServerPortNumber(equipInfo.standbyServerPortNumber)"> {{standbyServerPortNumberBtn}}</span>

                                </td>
                                <td style="background-color: #F6F7FB;padding-left: 3px;">脱&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;扣
                                    ：
                                </td>

                                <td class="pricrTd">{{equipInfo.relay==0?'关闭':'开启'}}</td>


                            </tr>
                            <tr v-if="this.routeParam.type=='0' ">

                                <td width="145px" style="background-color: #F6F7FB;">域名地址：</td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.domainNameAddress" :disabled="editDomainNameAddress"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setDomainNameAddress(equipInfo.domainNameAddress)"> {{domainNameAddressBtn}}</span>
                                </td>

                            </tr>
                            <tr v-if="this.routeParam.type=='1' || this.routeParam.type=='剩余电流' ">
                                <td width="145px" style="background-color: #F6F7FB;">电流互感器比例系数：</td>
                                <td class="pricrTd" style="padding: 0;text-indent: 0">
                                    <input type="text" v-model="equipInfo.currentTransformerProCoefficient" :disabled="editCurrentTransformerProCoefficient"
                                           style="width: calc(100% - 30px);border: 0;height: 100%;text-indent: 10px;overflow: hidden;text-overflow: ellipsis;white-space: nowrap"/>
                                    <span style="cursor: pointer; color: #2d8cf0; font-size: 10px;display: inline-block"
                                          @click="setCurrentTransformerProCoefficient(equipInfo.currentTransformerProCoefficient)"> {{CurrentTransformerProCoefficientBtn}}</span>
                                </td>
                                <td width="145px" style="background-color: #F6F7FB;">电压检测方式：</td>
                                <td>
                                <el-select  v-model="voltageDetectionTypeValue"  @change="clickVoltageDetection" >
                                    <el-option  v-for="item in voltageDetectionType" :key="item.value"
                                               :label="item.label" :value="item.value">
                                    </el-option>
                                </el-select>
                                </td>


                            <tr/>
                        </table>
                    </div>
                    <div v-if="this.routeParam.type=='1' || this.routeParam.type=='剩余电流' "
                         style="margin-top: 23px;margin-bottom: 240px">

                        <el-radio-group v-model="type" @change="changeParameterType" style="padding-left: 16px">
                            <el-radio-button label="0">电压告警</el-radio-button>
                            <el-radio-button label="1">电流告警</el-radio-button>
                            <el-radio-button label="2">温度告警</el-radio-button>
                            <el-radio-button label="3">其他告警</el-radio-button>
                        </el-radio-group>
                        <span style="margin-right: 20px;float: right">
                          告警使能总开关：
                          <el-switch v-model="alarmEnableMainSwitchValue"
                                     @change="setAlarmEnableSwitchValue()"> </el-switch>

                      </span>
                        <hr class="hrInline">

                        <input type="button" class="setLimitBtn" value="阈值设置" v-on:click="getParameterLimitInfo(1)">
                        <div style="margin-top: 0.5rem">
                            <div v-if="this.type==1" class="firstParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/current.png" alt=""></span>
                                    <span>A相电流(A)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.aCurrent}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="aCurrentWarningEnableValue"
                                               @change="setCurrentEnable(aCurrentWarningEnableValue,7)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>电流上限阈值(A)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.aCurrentUpLimit}}
                                    </span>
                                    </div>
                                </div>
                            </div>
                            <div v-if="this.type==1" class="secondParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/current.png" alt=""></span>
                                    <span>B相电流(A)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.bCurrent}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="bCurrentWarningEnableValue"
                                               @change="setCurrentEnable(bCurrentWarningEnableValue,6)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>电流上限阈值(A)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.bCurrentUpLimit}}
                                    </span>
                                    </div>
                                </div>

                            </div>
                            <div v-if="this.type==1" class="secondParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/current.png" alt=""></span>
                                    <span>C相电流(A)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.cCurrent}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="cCurrentWarningEnableValue"
                                               @change="setCurrentEnable(cCurrentWarningEnableValue,5)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>电流上限阈值(A)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.cCurrentUpLimit}}
                                    </span>
                                    </div>
                                </div>
                            </div>

                            <div v-if="this.type==0" class="firstParameter voltageHeight">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/voltage.png" alt=""></span>
                                    <span>A相电压(V)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.aVoltage}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="aVoltageWarningEnableValue"
                                               @change="setVoltageEnable(aVoltageWarningEnableValue,7)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>电压上限阈值(V)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.aVoltageUpLimit}}
                                    </span>
                                    </div>
                                    <div class="parameterUpLimit">
                                        <span>电压下限阈值(V)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.aVoltageDownLimit}}
                                    </span>
                                    </div>
                                </div>

                            </div>
                            <div v-if="this.type==0" class="secondParameter voltageHeight">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/voltage.png" alt=""></span>
                                    <span>B相电压(V)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.bVoltage}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="bVoltageWarningEnableValue"
                                               @change="setVoltageEnable(bVoltageWarningEnableValue,6)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>电压上限阈值(V)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.bVoltageUpLimit}}
                                    </span>
                                    </div>
                                    <div class="parameterUpLimit">
                                        <span>电压下限阈值(V)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.bVoltageDownLimit}}
                                    </span>
                                    </div>
                                </div>
                            </div>
                            <div v-if="this.type==0" class="secondParameter voltageHeight">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/voltage.png" alt=""></span>
                                    <span>C相电压(V)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.cVoltage}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="cVoltageWarningEnableValue"
                                               @change="setVoltageEnable(cVoltageWarningEnableValue,5)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>电压上限阈值(V)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.cVoltageUpLimit}}
                                    </span>
                                    </div>
                                    <div class="parameterUpLimit">
                                        <span>电压下限阈值(V)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.cVoltageDownLimit}}
                                    </span>
                                    </div>
                                </div>
                            </div>


                            <!--温度-->
                            <div v-if="this.type==2" class="firstParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/tem.png" alt=""></span>
                                    <span>处理器温度值(℃)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.tem0}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="tem0WarningEnableValue"
                                               @change="setTemEnable(tem0WarningEnableValue,7)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>温度门限(℃)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.temLimit0}}
                                    </span>
                                    </div>
                                </div>

                            </div>
                            <div v-if="this.type==2" class="secondParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/tem.png" alt=""></span>
                                    <span>1路温度值(℃)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.tem1}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="tem1WarningEnableValue"
                                               @change="setTemEnable(tem1WarningEnableValue,6)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>温度门限(℃)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.temLimit1}}
                                    </span>
                                    </div>
                                </div>
                            </div>
                            <div v-if="this.type==2" class="secondParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/tem.png" alt=""></span>
                                    <span>2路温度值(℃)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.tem2}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="tem2WarningEnableValue"
                                               @change="setTemEnable(tem2WarningEnableValue,5)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>温度门限(℃)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.temLimit2}}
                                    </span>
                                    </div>
                                </div>
                            </div>

                            <div v-if="this.type==2" class="firstParameter thirdParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/tem.png" alt=""></span>
                                    <span>3路温度值(℃)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.tem3}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="tem3WarningEnableValue"
                                               @change="setTemEnable(tem3WarningEnableValue,4)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>温度门限(℃)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.temLimit3}}
                                    </span>
                                    </div>
                                </div>
                            </div>
                            <div v-if="this.type==2" class="secondParameter thirdParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/tem.png" alt=""></span>
                                    <span>4路温度值(℃)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.tem4}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="tem4WarningEnableValue"
                                               @change="setTemEnable(tem4WarningEnableValue,3)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>温度门限(℃)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.temLimit4}}
                                    </span>
                                    </div>
                                </div>
                            </div>
                            <!--其他告警-->
                            <div v-if="this.type==3" class="firstParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/other.png" alt=""></span>
                                    <span>漏电流(mA)</span>
                                    <span class="parameterEnable">告警使能</span>
                                </div>
                                <div class="parameterMiddle">
                                    <span class="parameterValue">{{equipInfo.leakageCurrent}}</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="leakageWarningEnableValue"
                                               @change="setOtherEnable(leakageWarningEnableValue,7)">
                                     </el-switch>
                                </span>
                                    <div class="parameterUpLimit">
                                        <span>漏电上限阀值(mA)</span>
                                        <span class="parameterUpLimitValue">
                                    {{equipInfo.leakageCurrentUpLimit}}
                                    </span>
                                    </div>
                                </div>

                            </div>
                            <div v-if="this.type==3" class="secondParameter">
                                <div class="parameterTop"></div>
                                <div>
                                    <span class="parameterImg"><img src="../../../assets/img/other.png" alt=""></span>
                                    <span>门禁告警使能</span>
                                    <span class="parameterEnableValue">
                                    <el-switch v-model="entranceWarningEnableValue"
                                               @change="setTemEnable(entranceWarningEnableValue,5)">
                                     </el-switch>
                                </span>
                                </div>

                            </div>
                        </div>

                    </div>


                    <div style="width:128px; height:34px; margin:80px auto auto auto;">
                        <div style="width:56px; float: left; height:34px;">
                            <el-button class="v-1-0-0-Button" @click="getInfo" :disabled="disabledType">查询</el-button>
                        </div>

                        <div style="width:56px; float: left; height:34px; margin-left: 16px; ">
                            <el-button class="v-1-0-0-Button-Minor" @click="goBack">返回</el-button>
                        </div>

                    </div>
                </div>
                <!-- 门限参数设置-->
               <div v-if="isSetLimit== 1">
                   <!-- 电压门限参数设置-->
                   <div v-if="type== 0">
                       <div class="limitDiv">
                       <span>A相电压上限阀值(V):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.aVoltageUpLimit"></div>
                       <div class="limitDiv">
                       <span>B相电压上限阀值(V):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.bVoltageUpLimit"></div>
                       <div class="limitDiv">
                       <span>C相电压上限阀值(V):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.cVoltageUpLimit"></div>
                       <div class="limitDiv">
                       <span>A相电压下限阀值(V):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.aVoltageDownLimit"></div>
                       <div class="limitDiv">
                       <span>B相电压下限阀值(V):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.bVoltageDownLimit"></div>
                       <div class="limitDiv">
                       <span>C相电压下限阀值(V):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.cVoltageDownLimit"></div>

                   </div>
                   <!-- 电流门限参数设置-->
                   <div v-if="type== 1">
                       <div class="limitDiv">
                       <span>A相电流上限阀值(A):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.aCurrentUpLimit"></div>
                       <div class="limitDiv">
                       <span>B相电流上限阀值(A):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.bCurrentUpLimit"></div>
                       <div class="limitDiv">
                       <span>C相电流上限阀值(A):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.cCurrentUpLimit"></div>

                   </div>
                   <!-- 温度门限参数设置-->
                   <div v-if="type== 2">
                       <div class="limitDiv">
                       <span>处理器温度门限(℃):</span>
                       <input class="limitInput" style="margin-left: 0.6rem" type="text" v-model="equipInfo.temLimit0"></div>
                       <div class="limitDiv">
                       <span>1路温度门限(℃):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.temLimit1"></div>
                       <div class="limitDiv">
                       <span>2路温度门限(℃):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.temLimit2"></div>
                       <div class="limitDiv">
                       <span>3路温度门限(℃):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.temLimit3"></div>
                       <div class="limitDiv">
                       <span>4路温度门限(℃):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.temLimit4"></div>
                   </div>
                   <!-- 其他门限参数设置-->
                   <div v-if="type== 3">
                       <div class="limitDiv">
                       <span>漏电上限阀值(mA):</span>
                       <input class="limitInput" type="text" v-model="equipInfo.leakageCurrentUpLimit"></div>

                   </div>


                   <div v-if="type==2" style="width:128px; height:34px; margin-top:1.5rem;margin-left: 11rem">
                       <div style="width:56px; float: left; height:34px;">
                           <el-button class="v-1-0-0-Button" @click="setLimit">保存</el-button>
                       </div>

                       <div style="width:56px; float: left; height:34px; margin-left: 16px; ">
                           <el-button class="v-1-0-0-Button-Minor" @click="gobackInfo">返回</el-button>
                       </div>

                   </div>
                   <div v-if="type==0"  style="width:128px; height:34px; margin-top:1.5rem;margin-left: 12.7rem">
                       <div style="width:56px; float: left; height:34px;">
                           <el-button class="v-1-0-0-Button" @click="setLimit">保存</el-button>
                       </div>

                       <div style="width:56px; float: left; height:34px; margin-left: 16px; ">
                           <el-button class="v-1-0-0-Button-Minor" @click="gobackInfo">返回</el-button>
                       </div>

                   </div>
                   <div v-if="type==1" style="width:128px; height:34px; margin-top:1.5rem;margin-left: 12.7rem">
                       <div style="width:56px; float: left; height:34px;">
                           <el-button class="v-1-0-0-Button" @click="setLimit">保存</el-button>
                       </div>

                       <div style="width:56px; float: left; height:34px; margin-left: 16px; ">
                           <el-button class="v-1-0-0-Button-Minor" @click="gobackInfo">返回</el-button>
                       </div>

                   </div>
                   <div v-if="type==3"  style="width:128px; height:34px; margin-top:1.5rem;margin-left: 11.8rem">
                       <div style="width:56px; float: left; height:34px;">
                           <el-button class="v-1-0-0-Button" @click="setLimit">保存</el-button>
                       </div>

                       <div style="width:56px; float: left; height:34px; margin-left: 16px; ">
                           <el-button class="v-1-0-0-Button-Minor" @click="gobackInfo">返回</el-button>
                       </div>

                   </div>
               </div>
            </div>
        </div>
    </div>
</template>

<script>
    import http from '../../../common/http';

    export default {
        name: "mqttInformation",
        data() {
            return {
                isSetLimit: 0,
                type: 0,
                routeParam: {},
                equipInfo: {},
                editPlace: true,
                editMsg: true,
                editLng: true,
                editLat: true,
                editNbNumber: true,
                editIccid: true,
                editIp: true,
                editPort: true,
                editLeakageCurrentUpLimit: true,
                editAVoltageUpLimit: true,
                editBVoltageUpLimit: true,
                editCVoltageUpLimit: true,
                editAVoltageDownLimit: true,
                editBVoltageDownLimit: true,
                editCVoltageDownLimit: true,
                editTemLimit0: true,
                editTemLimit1: true,
                editTemLimit2: true,
                editTemLimit3: true,
                editTemLimit4: true,
                editACurrentUpLimit: true,
                editBCurrentUpLimit: true,
                editCCurrentUpLimit: true,
                editArcWarningSensitivity: true,
                editSeartbeatTime: true,
                editStandbyServerIpAddress: true,
                editStandbyServerPortNumber: true,
                editDomainNameAddress: true,
                editCurrentTransformerProCoefficient: true,
                placeBtn: "修改",
                msgBtn: "修改",
                lngBtn: "修改",
                latBtn: "修改",
                nbNumberBtn: "修改",
                iccidBtn: "修改",
                ipBtn: "修改",
                portBtn: "修改",
                temLimit0Btn: "修改",
                temLimit1Btn: "修改",
                temLimit2Btn: "修改",
                temLimit3Btn: "修改",
                temLimit4Btn: "修改",
                aCurrentUpLimitBtn: "修改",
                bCurrentUpLimitBtn: "修改",
                cCurrentUpLimitBtn: "修改",
                aVoltageUpLimitBtn: "修改",
                bVoltageUpLimitBtn: "修改",
                cVoltageUpLimitBtn: "修改",
                aVoltageDownLimitBtn: "修改",
                bVoltageDownLimitBtn: "修改",
                cVoltageDownLimitBtn: "修改",
                leakageCurrentUpLimitBtn: "修改",
                arcWarningSensitivityBtn: "修改",
                seartbeatTimeBtn: "修改",
                standbyServerPortNumberBtn: "修改",
                standbyServerIpAddressBtn: "修改",
                domainNameAddressBtn: "修改",
                CurrentTransformerProCoefficientBtn: "修改",


                buzzerEnableValue: false,
                arcAlarmEnableValue: false,
                aCurrentWarningEnableValue: false,
                bCurrentWarningEnableValue: false,
                cCurrentWarningEnableValue: false,
                aVoltageWarningEnableValue: false,
                bVoltageWarningEnableValue: false,
                cVoltageWarningEnableValue: false,
                tem0WarningEnableValue: false,
                tem1WarningEnableValue: false,
                tem2WarningEnableValue: false,
                tem3WarningEnableValue: false,
                tem4WarningEnableValue: false,
                leakageWarningEnableValue: false,
                entranceWarningEnableValue: false,
                alarmEnableMainSwitchValue: false,
                //设备类型数组
                voltageDetectionTypeValue:"未设置",
                disabledType:false,
                voltageDetectionType: [
                    {
                        label: "三相四线",
                        value: "00"
                    },
                    {
                        label: "三相三线",
                        value: "01"
                    },
                    {
                        label: "单相",
                        value: "02"
                    },
                    {
                        label:"关闭",
                        value:"03"
                    },
                    {
                        label:"未设置",
                        value:"FF"
                    },
                ],
            }
        },
        created: function () {
            this.routeParam = this.$route.params;
            console.log(this.routeParam);
            this.getEquipInfo();
        },
        methods: {

            clickVoltageDetection(){
                let  cmd='voltageDetection';
                let content=this.voltageDetectionTypeValue;
                console.info(content,"content");
                this.setParams(cmd, content);

            },
            //进入阈值设置
            getParameterLimitInfo(isSetLimit){
                this.isSetLimit=isSetLimit;
            },
            //切换告警类型
            changeParameterType(ParameterType) {
                this.type = ParameterType;
            },
            gobackInfo(){
                this.isSetLimit=0;
            },
            setLimit(){
              if(this.type==0){
                  let cmd="AVoltageUpLimit,BVoltageUpLimit,CVoltageUpLimit,AVoltageDownLimit,BVoltageDownLimit,CVoltageDownLimit";
                  let content=this.equipInfo.aVoltageUpLimit+","+this.equipInfo.bVoltageUpLimit+","+this.equipInfo.cVoltageUpLimit+","+this.equipInfo.aVoltageDownLimit+","+this.equipInfo.bVoltageDownLimit+","+this.equipInfo.cVoltageDownLimit
                 console.log("cmd",cmd);
                  console.log("content",content);
                  this.setParams(cmd, content);
              }
              if(this.type==1){
                  let cmd="ACurrentUpLimit,BCurrentUpLimit,CCurrentUpLimit";
                  let content=this.equipInfo.aCurrentUpLimit+","+this.equipInfo.bCurrentUpLimit+","+this.equipInfo.cCurrentUpLimit
                  this.setParams(cmd, content);
              }
              if(this.type==2){
                    let cmd="0temLimit,1temLimit,2temLimit,3temLimit,4temLimit";
                    let content=this.equipInfo.temLimit0+","+this.equipInfo.temLimit1+","+this.equipInfo.temLimit2+","+this.equipInfo.temLimit3+","+this.equipInfo.temLimit4
                  this.setParams(cmd, content);
              }
              if(this.type==3){
                    let cmd="leakageCurrentUpLimit";
                    let content=this.equipInfo.leakageCurrentUpLimit
                  this.setParams(cmd, content);
              }
              setTimeout(this.gobackInfo,3000);
            },
            //查询
            getInfo() {
                if (this.routeParam.type == '1' || this.routeParam.type == '剩余电流') {
                    this.queryMqtt2();
                    setTimeout(this.queryMqtt5, 1500);
                    setTimeout(this.queryMqtt, 3000);
                } else {
                    this.queryMqtt4();
                    setTimeout(this.queryMqtt3, 3000);

                }
                this.disabledType=true;
               setTimeout(this.setTimeForButton,3000);
            },
            setTimeForButton(){
              this.disabledType=false;
            },
            getTest() {
                this.setParams("fire_alarm", "01");
            },
            getReset() {
                this.setParams("alarm_reset", "01");
            },
            changePlace() {
                if (this.editPlace == true) {
                    this.editPlace = false;
                    this.placeBtn = "确定";
                } else {
                    this.changeParam();
                }
            },

            setSeartbeatTime(content) {
                if (this.editSeartbeatTime == true) {
                    this.editSeartbeatTime = false;
                    this.seartbeatTimeBtn = "确定";
                } else {
                    this.setParams('seartbeat_time', content);
                }
            },
            setDomainNameAddress(content) {
                if (this.editDomainNameAddress == true) {
                    this.editDomainNameAddress = false;
                    this.domainNameAddressBtn = "确定";
                } else {
                    this.setParams('domainNameAddress', content);
                }
            },
            setCurrentTransformerProCoefficient(content) {
                if (this.editCurrentTransformerProCoefficient == true){
                    this.editCurrentTransformerProCoefficient = false;
                    this.CurrentTransformerProCoefficientBtn = "确定";
                } else {
                    this.setParams("CurrentTransformerProCoefficient",content);
                }
            },
            setArcWarningSensitivity(content) {
                if (this.editArcWarningSensitivity == true) {
                    this.editArcWarningSensitivity = false;
                    this.arcWarningSensitivityBtn = "确定";
                } else {
                    this.setParams('arc_warning_sensitivity', content);
                }
            },
            setMsg(content) {
                if (this.editMsg == true) {
                    this.editMsg = false;
                    this.msgBtn = "确定";
                } else {
                    this.setParams('device_descriptor', content);
                }
            },
            setLeakageCurrentUpLimit(content) {
                if (this.editLeakageCurrentUpLimit == true) {
                    this.editLeakageCurrentUpLimit = false;
                    this.leakageCurrentUpLimitBtn = "确定";
                } else {
                    this.setParams('leakageCurrentUpLimit', content);
                }
            },
            setAVoltageDownLimit(content) {
                if (this.editAVoltageDownLimit == true) {
                    this.editAVoltageDownLimit = false;
                    this.aVoltageDownLimitBtn = "确定";
                } else {
                    this.setParams('AVoltageDownLimit', content);
                }
            },
            setBVoltageDownLimit(content) {
                if (this.editBVoltageDownLimit == true) {
                    this.editBVoltageDownLimit = false;
                    this.bVoltageDownLimitBtn = "确定";
                } else {
                    this.setParams('BVoltageDownLimit', content);
                }
            },
            setCVoltageDownLimit(content) {
                if (this.editCVoltageDownLimit == true) {
                    this.editCVoltageDownLimit = false;
                    this.cVoltageDownLimitBtn = "确定";
                } else {
                    this.setParams('CVoltageDownLimit', content);
                }
            },
            setAVoltageUpLimit(content) {
                if (this.editAVoltageUpLimit == true) {
                    this.editAVoltageUpLimit = false;
                    this.aVoltageUpLimitBtn = "确定";
                } else {
                    this.setParams('AVoltageUpLimit', content);
                }
            },
            setBVoltageUpLimit(content) {
                if (this.editBVoltageUpLimit == true) {
                    this.editBVoltageUpLimit = false;
                    this.bVoltageUpLimitBtn = "确定";
                } else {
                    this.setParams('BVoltageUpLimit', content);
                }
            },
            setCVoltageUpLimit(content) {
                if (this.editCVoltageUpLimit == true) {
                    this.editCVoltageUpLimit = false;
                    this.cVoltageUpLimitBtn = "确定";
                } else {
                    this.setParams('CVoltageUpLimit', content);
                }
            },
            setACurrentUpLimit(content) {
                if (this.editACurrentUpLimit == true) {
                    this.editACurrentUpLimit = false;
                    this.aCurrentUpLimitBtn = "确定";
                } else {
                    this.setParams('ACurrentUpLimit', content);
                }
            },
            setBCurrentUpLimit(content) {
                if (this.editBCurrentUpLimit == true) {
                    this.editBCurrentUpLimit = false;
                    this.bCurrentUpLimitBtn = "确定";
                } else {
                    this.setParams('BCurrentUpLimit', content);
                }
            },
            setCCurrentUpLimit(content) {
                if (this.editCCurrentUpLimit == true) {
                    this.editCCurrentUpLimit = false;
                    this.cCurrentUpLimitBtn = "确定";
                } else {
                    this.setParams('CCurrentUpLimit', content);
                }
            },
            setTemLimit0(content) {
                if (this.editTemLimit0 == true) {
                    this.editTemLimit0 = false;
                    this.temLimit0Btn = "确定";
                } else {
                    this.setParams('0temLimit', content);
                }
            },
            setTemLimit1(content) {
                if (this.editTemLimit1 == true) {
                    this.editTemLimit1 = false;
                    this.temLimit1Btn = "确定";
                } else {
                    this.setParams('1temLimit', content);
                }
            },
            setTemLimit2(content) {
                if (this.editTemLimit2 == true) {
                    this.editTemLimit2 = false;
                    this.temLimit2Btn = "确定";
                } else {
                    this.setParams('2temLimit', content);
                }
            },
            setTemLimit3(content) {
                if (this.editTemLimit3 == true) {
                    this.editTemLimit3 = false;
                    this.temLimit3Btn = "确定";
                } else {
                    this.setParams('3temLimit', content);
                }
            },
            setTemLimit4(content) {
                if (this.editTemLimit4 == true) {
                    this.editTemLimit4 = false;
                    this.temLimit4Btn = "确定";
                } else {
                    this.setParams('4temLimit', content);
                }
            },

            setMsg(content) {
                if (this.editMsg == true) {
                    this.editMsg = false;
                    this.msgBtn = "确定";
                } else {
                    this.setParams('device_descriptor', content);
                }
            },
            setLng(content) {
                if (this.editLng == true) {
                    this.editLng = false;
                    this.lngBtn = "确定";
                } else {
                    this.setParams('longitude', content);
                }
            },
            setLat(content) {
                if (this.editLat == true) {
                    this.editLat = false;
                    this.latBtn = "确定";
                } else {
                    this.setParams('latitude', content);
                }
            },
            setNbNumber(content) {
                if (this.editNbNumber == true) {
                    this.editNbNumber = false;
                    this.nbNumberBtn = "确定";
                } else {
                    this.setParams('nb_phone_number', content);
                }
            },
            setIccid(content) {
                if (this.editIccid == true) {
                    this.editIccid = false;
                    this.iccidBtn = "确定";
                } else {
                    this.setParams('nb_chip_serial_number', content);
                }
            },
            setIp(content) {
                if (this.editIp == true) {
                    this.editIp = false;
                    this.ipBtn = "确定";
                } else {
                    let ipArr = content.split(".");
                    if (ipArr.length != 4) {
                        this.$commonFn.showTip("格式错误", 2);
                        return;
                    }
                    let ipStr = "";
                    for (let i = 0; i < ipArr.length; i++) {
                        let str = parseInt(ipArr[i]).toString(16);
                        if (str.length == 1) {
                            str = "0" + str;
                        }
                        ipStr += str;
                    }
                    ipStr = ipStr.toUpperCase();
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'server_ip_address',
                        qos: 1,
                        ID: "01" + this.equipInfo.id,
                        type: 3,
                        extraData: ipStr
                    };
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("设置成功", 1);
                        this.editIp = true;
                        this.ipBtn = "修改";
                        setTimeout(this.getEquipInfo, 5000);
                    }, (err) => {
                        console.log(err);
                        this.editIp = true;
                        this.ipBtn = "修改";
                    });
                }
            },
            setStandbyServerIpAddress(content) {
                if (this.editStandbyServerIpAddress == true) {
                    this.editStandbyServerIpAddress = false;
                    this.standbyServerIpAddressBtn = "确定";
                } else {
                    let ipArr = content.split(".");
                    if (ipArr.length != 4) {
                        this.$commonFn.showTip("格式错误", 2);
                        return;
                    }
                    let ipStr = "";
                    for (let i = 0; i < ipArr.length; i++) {
                        let str = parseInt(ipArr[i]).toString(16);
                        if (str.length == 1) {
                            str = "0" + str;
                        }
                        ipStr += str;
                    }
                    ipStr = ipStr.toUpperCase();
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'standby_server_ip_address',
                        qos: 1,
                        ID: "01" + this.equipInfo.id,
                        type: 3,
                        extraData: ipStr
                    };
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("设置成功", 1);
                        this.editStandbyServerIpAddress = true;
                        this.standbyServerIpAddressBtn = "修改";
                        setTimeout(this.getEquipInfo, 5000);
                    }, (err) => {
                        console.log(err);
                        this.editStandbyServerIpAddress = true;
                        this.standbyServerIpAddressBtn = "修改";
                    });
                }
            },
            setBuzzerEnable() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    message: 'buzzer_enable',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: "0" + Number(!this.buzzerEnableValue)
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            setAlarmEnableSwitchValue() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    message: 'alarmEnableMainSwitch',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: "0" + Number(!this.alarmEnableMainSwitchValue)
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            setArcAlarmEnable() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    message: 'arc_alarm_enable',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: "0" + Number(!this.arcAlarmEnableValue)
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            //改变电流使能的参数
            setCurrentEnable(enableValue, index) {
                let extraData = "00000";
                if (index == 7) {
                    extraData = extraData + this.equipInfo.ccurrentWarningEnable + this.equipInfo.bcurrentWarningEnable + Number(!enableValue);
                }
                if (index == 6) {
                    extraData = extraData + this.equipInfo.ccurrentWarningEnable + Number(!enableValue) + this.equipInfo.acurrentWarningEnable;
                }
                if (index == 5) {
                    extraData = extraData + Number(!enableValue) + this.equipInfo.bcurrentWarningEnable + this.equipInfo.acurrentWarningEnable;
                }
                console.log("extraData" + extraData);
                //2进制转16进制补0
                extraData = this.bin_to_hex(extraData);
                if (extraData.length == 1) {
                    extraData = "0" + extraData;
                }
                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message: "CurrentWarnEnable",
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: extraData
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            //改变电压使能的参数
            setVoltageEnable(enableValue, index) {
                let extraData = "00000";
                if (index == 7) {
                    extraData = extraData + this.equipInfo.cvoltageWarningEnable + this.equipInfo.bvoltageWarningEnable + Number(!enableValue);
                }
                if (index == 6) {
                    extraData = extraData + this.equipInfo.cvoltageWarningEnable + Number(!enableValue) + this.equipInfo.avoltageWarningEnable;
                }
                if (index == 5) {
                    extraData = extraData + Number(!enableValue) + this.equipInfo.bvoltageWarningEnable + this.equipInfo.avoltageWarningEnable;
                }
                //2进制转16进制补0
                extraData = this.bin_to_hex(extraData);
                if (extraData.length == 1) {
                    extraData = "0" + extraData;
                }
                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message: "VoltageWarnEnable",
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: extraData
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            //改变温度使能的参数
            setTemEnable(enableValue, index) {
                let extraData = "000";
                if (index == 7) {
                    extraData = extraData + this.equipInfo.temperatureWarning4Enable + this.equipInfo.temperatureWarning3Enable + this.equipInfo.temperatureWarning2Enable + this.equipInfo.temperatureWarning1Enable + Number(!enableValue);
                }
                if (index == 6) {
                    extraData = extraData + this.equipInfo.temperatureWarning4Enable + this.equipInfo.temperatureWarning3Enable + this.equipInfo.temperatureWarning2Enable + Number(!enableValue) + this.equipInfo.temperatureWarning0Enable;
                }
                if (index == 5) {
                    extraData = extraData + this.equipInfo.temperatureWarning4Enable + this.equipInfo.temperatureWarning3Enable + Number(!enableValue) + this.equipInfo.temperatureWarning1Enable + this.equipInfo.temperatureWarning0Enable;
                }
                if (index == 4) {
                    extraData = extraData + this.equipInfo.temperatureWarning4Enable + Number(!enableValue) + this.equipInfo.temperatureWarning2Enable + this.equipInfo.temperatureWarning1Enable + this.equipInfo.temperatureWarning0Enable;
                }
                if (index == 3) {
                    extraData = extraData + Number(!enableValue) + this.equipInfo.temperatureWarning3Enable + this.equipInfo.temperatureWarning2Enable + this.equipInfo.temperatureWarning1Enable + this.equipInfo.temperatureWarning0Enable;
                }
                //2进制转16进制补0
                extraData = this.bin_to_hex(extraData);
                if (extraData.length == 1) {
                    extraData = "0" + extraData;
                }
                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message: "TemWarnEnable",
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: extraData
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            //改变其他使能的参数
            setOtherEnable(enableValue, index) {
                let extraData = "000000";
                if (index == 7) {
                    extraData = extraData + this.equipInfo.entranceWarningEnable + Number(!enableValue);
                }
                if (index == 6) {
                    extraData = extraData + Number(!enableValue) + this.equipInfo.leakageWarningEnable;
                }

                //2进制转16进制补0
                extraData = this.bin_to_hex(extraData);
                if (extraData.length == 1) {
                    extraData = "0" + extraData;
                }
                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message: "OtherWarnEnable",
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: extraData
                };
                this.apiPost(url, data).then((res) => {
                    setTimeout(this.getEquipInfo, 1500);
                }, (err) => {

                });
            },
            setPort(content) {
                if (this.editPort == true) {
                    this.editPort = false;
                    this.portBtn = "确定";
                } else {
                    let port = content;
                    let dport = this.decimalToHexString(parseInt(port));
                    if (dport.length != 4) {
                        dport = "0" + dport
                    }
                    let dport1 = dport.substring(0, 2);
                    let dport2 = dport.substring(2, 4);
                    dport = dport2 + dport1;
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'server_port_number',
                        qos: 1,
                        ID: "01" + this.equipInfo.id,
                        type: 3,
                        extraData: dport
                    };
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("设置成功", 1);
                        this.editPort = true;
                        this.portBtn = "修改";
                        setTimeout(this.getEquipInfo, 5000);
                    }, (err) => {
                        this.editPort = true;
                        this.portBtn = "修改";
                        console.log(err);
                    });
                }
            },
            setStandbyServerPortNumber(content) {
                if (this.editStandbyServerPortNumber == true) {
                    this.editStandbyServerPortNumber = false;
                    this.standbyServerPortNumberBtn = "确定";
                } else {
                    let port = content;
                    let dport = this.decimalToHexString(parseInt(port));
                    if (dport.length != 4) {
                        dport = "0" + dport
                    }
                    let dport1 = dport.substring(0, 2);
                    let dport2 = dport.substring(2, 4);
                    dport = dport2 + dport1;
                    let url = this.mqttIssuingInstructionsUrl;
                    let data = {
                        message: 'standby_server_port_number',
                        qos: 1,
                        ID: "01" + this.equipInfo.id,
                        type: 3,
                        extraData: dport
                    };
                    this.apiPost(url, data).then((res) => {
                        this.$commonFn.showTip("设置成功", 1);
                        this.editStandbyServerPortNumber = true;
                        this.standbyServerPortNumberBtn = "修改";
                        setTimeout(this.getEquipInfo, 5000);
                    }, (err) => {
                        this.editStandbyServerPortNumber = true;
                        this.standbyServerPortNumberBtn = "修改";
                        console.log(err);
                    });
                }
            },
            decimalToHexString(number) {
                if (number < 0) {
                    number = 0xFFFFFFFF + number + 1;
                }
                return number.toString(16).toUpperCase();
            },
            changeParam() {
                let url = this.editEquipInfoUrl;
                let data = {
                    equipId: this.equipInfo.id,
                    equipPlace: this.equipInfo.equip_place
                };
                this.apiPost(url, data).then((res) => {
                    this.editPlace = true;
                    this.placeBtn = "修改";
                });
            },
            setParams(cmd, content) {

                let url = this.setMqttIssuingInstructionsUrl;
                let data = {
                    message: cmd,
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 3,
                    extraData: content
                };

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
                    else if (cmd == 'domainNameAddress') {
                        this.editDomainNameAddress = true;
                        this.domainNameAddressBtn = "修改";
                    }
                    else if(cmd == 'CurrentTransformerProCoefficient'){
                        this.editCurrentTransformerProCoefficient = true;
                        this.CurrentTransformerProCoefficientBtn = "修改";
                    }
                    setTimeout(this.getEquipInfo, 5000);
                });
            },
            getEquipInfo() {
                let url = this.getEquipInfoUrl;
                let data = {
                    equipId: this.routeParam.id
                };
                this.apiPost(url, data).then((res) => {

                    if (res.status === 200) {
                        this.equipInfo = res.data;
                        console.log(this.equipInfo)
                        this.equipInfo.serialNumber = parseInt(this.equipInfo.line_code) + 1 + "-" + parseInt(this.equipInfo.addr);
                        let typeArr = ['电弧探测器', '组合式探测器', '测温式探测器'];
                        this.equipInfo.type = typeArr[this.equipInfo.type];
                        this.equipInfo.siteLabel = this.equipInfo.siteName ? this.equipInfo.siteName : this.equipInfo.siteCode;
                        if (this.equipInfo.niNameDevice) {
                            this.equipInfo.efmLabel = this.equipInfo.niNameDevice;
                        } else if (this.equipInfo.specificatorDevice) {
                            this.equipInfo.efmLabel = this.equipInfo.specificatorDevice;
                        } else {
                            this.equipInfo.efmLabel = parseInt(this.equipInfo.deviceCode, 16);
                        }
                        this.buzzerEnableValue = !this.equipInfo.buzzerEnable;
                        this.arcAlarmEnableValue = !this.equipInfo.arcAlarmEnable;
                        for (let i = 0; i <this.voltageDetectionType.length ; i++) {
                            if(this.equipInfo.voltageDetectionValue==this.voltageDetectionType[i].value || "0"+this.equipInfo.voltageDetectionValue == this.voltageDetectionType[i].value){
                                console.info("hhhhaaa",this.voltageDetectionType[i].label,this.voltageDetectionType[i].value);
                                this.voltageDetectionTypeValue= this.voltageDetectionType[i].label;
                            }
                        };
                        this.aCurrentWarningEnableValue = this.equipInfo.acurrentWarningEnable == 0 ? true : false;
                        this.bCurrentWarningEnableValue = this.equipInfo.bcurrentWarningEnable == 0 ? true : false;
                        this.cCurrentWarningEnableValue = this.equipInfo.ccurrentWarningEnable == 0 ? true : false;

                        this.aVoltageWarningEnableValue = this.equipInfo.avoltageWarningEnable == 0 ? true : false;
                        this.bVoltageWarningEnableValue = this.equipInfo.bvoltageWarningEnable == 0 ? true : false;
                        this.cVoltageWarningEnableValue = this.equipInfo.cvoltageWarningEnable == 0 ? true : false;

                        this.tem0WarningEnableValue = this.equipInfo.temperatureWarning0Enable == 0 ? true : false;
                        this.tem1WarningEnableValue = this.equipInfo.temperatureWarning1Enable == 0 ? true : false;
                        this.tem2WarningEnableValue = this.equipInfo.temperatureWarning2Enable == 0 ? true : false;
                        this.tem3WarningEnableValue = this.equipInfo.temperatureWarning3Enable == 0 ? true : false;
                        this.tem4WarningEnableValue = this.equipInfo.temperatureWarning4Enable == 0 ? true : false;

                        this.leakageWarningEnableValue = this.equipInfo.leakageWarningEnable == 0 ? true : false;
                        this.entranceWarningEnableValue = this.equipInfo.entranceWarningEnable == 0 ? true : false;
                        this.alarmEnableMainSwitchValue = this.equipInfo.alarmEnableMainSwitch == 0 ? true : false;

                    }
                });
            },
            checkTerminalAlarm() {
                this.$router.push(
                    {
                        name: 'terminalAlarm',
                        params: {
                            equipId: this.routeParam.equipId,
                            efmId: this.routeParam.efmId,
                            efmName: this.routeParam.efmName,
                            siteLabel: this.routeParam.efmName.siteLabel
                        }
                    }
                );
            },
            //查询mqtt参数
            queryMqtt() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    message: 'equipment_category,equipment_type,arc_number,arc_time_range,hardware_version,issue_time,equipment_production_sequence_number,software_version,seartbeat_time,tem0,tem1,tem3,tem3,tem4,leakageCurrent,CCurrent,BCurrent,ACurrent,AVoltage,BVoltage,CVoltage',
                    //message: 'equipment_type,arc_number,arc_time_range,temperature_warning_threshold,hardware_version,longitude,latitude',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 5,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    this.$commonFn.showTip("查询成功", 1);
                    setTimeout(this.getEquipInfo, 2000);
                }, (err) => {
                    console.log(err);
                });
            },
            queryMqtt2() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    //message:'standby_server_ip_address,standby_server_port_number',
                    message: 'longitude,latitude,device_descriptor,nb_chip_serial_number,nb_phone_number,server_ip_address,server_port_number,buzzer_enable,0temLimit,1temLimit,2temLimit,3temLimit,4temLimit,leakageCurrentUpLimit,AVoltageUpLimit,BVoltageUpLimit,CVoltageUpLimit,' +
                        'AVoltageDownLimit,BVoltageDownLimit,CVoltageDownLimit,ACurrentUpLimit,BCurrentUpLimit,CCurrentUpLimit,standby_server_ip_address,standby_server_port_number',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 5,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    //this.$commonFn.showTip("查询成功", 1);
                }, (err) => {
                    console.log(err);
                });
            },
            queryMqtt3() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    //message: 'software_version',
                    message: 'longitude,latitude,device_descriptor,nb_chip_serial_number,server_ip_address,server_port_number,equipment_production_sequence_number,seartbeat_time,buzzer_enable',
                    //message: 'equipment_type,arc_number,arc_time_range,temperature_warning_threshold,hardware_version,longitude,latitude',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 5,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    this.$commonFn.showTip("查询成功", 1);
                    // this.getEquipInfo();
                    setTimeout(this.getEquipInfo, 2000);
                }, (err) => {
                    console.log(err);
                });
            },
            queryMqtt4() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    //domainNameAddress
                    message: 'software_version,arc_warning_sensitivity,0temLimit,equipment_type,hardware_version,standby_server_ip_address,standby_server_port_number,arc_alarm_enable,domainNameAddress,' +
                        '',

                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 5,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    //this.$commonFn.showTip("查询成功", 1);
                }, (err) => {
                    console.log(err);
                });
            },
            queryMqtt5() {
                let url = this.mqttIssuingInstructionsUrl;
                let data = {
                    message: 'OtherWarnEnable,VoltageWarnEnable,CurrentWarnEnable,TemWarnEnable,alarmEnableMainSwitch,domainNameAddress,voltageDetection,CurrentTransformerProCoefficient',
                    qos: 1,
                    ID: "01" + this.equipInfo.id,
                    type: 5,
                    extraData: ""
                };
                this.apiPost(url, data).then((res) => {
                    //this.$commonFn.showTip("查询成功", 1);
                }, (err) => {
                    console.log(err);
                });
            },
            goBack() {
                this.$router.push(
                    {
                        name: 'mqttDeviceList',
                        params: {
                            siteId: this.routeParam.site_id,
                            addType: this.routeParam.addType,
                            inputBoxValue: this.routeParam.inputBoxValue,
                            page: this.routeParam.page
                        }
                    }
                );
            },
            //2进制转16进制
            bin_to_hex(str) {

                let hex_array = [{key: 0, val: "0000"}, {key: 1, val: "0001"}, {key: 2, val: "0010"}, {
                    key: 3,
                    val: "0011"
                }, {key: 4, val: "0100"}, {key: 5, val: "0101"}, {key: 6, val: "0110"}, {key: 7, val: "0111"},
                    {key: 8, val: "1000"}, {key: 9, val: "1001"}, {key: 'a', val: "1010"}, {
                        key: 'b',
                        val: "1011"
                    }, {key: 'c', val: "1100"}, {key: 'd', val: "1101"}, {key: 'e', val: "1110"}, {
                        key: 'f',
                        val: "1111"
                    }]
                let value = ''
                let list = []

                if (str.length % 4 !== 0) {
                    let a = "0000"
                    let b = a.substring(0, 4 - str.length % 4)
                    str = b.concat(str)
                }
                console.log(str)
                while (str.length > 4) {
                    list.push(str.substring(0, 4))
                    str = str.substring(4);
                }
                list.push(str)
                console.log(list)
                for (let i = 0; i < list.length; i++) {
                    for (let j = 0; j < hex_array.length; j++) {
                        if (list[i] == hex_array[j].val) {
                            value = value.concat(hex_array[j].key)
                            break
                        }
                    }
                }

                return value
            },
        },
        mixins: [http]
    }

</script>

<style scoped>
    table {
        border: 1px solid rgb(216, 220, 228);
        border-collapse: collapse;
    }

    td {
        font-weight: 400;
        text-align: center;
        border: 1px solid rgb(216, 220, 228);
        height: 38px;
    }

    .pricrTd {
        font-weight: 400 !important;
        text-align: left !important;
        text-indent: 10px;
        width: 170px;
    }

    .buttonClass {

        margin-top: 400px !important;
    }

    .hrInline {
        /*width: 966px;*/
        height: 1px;
        background: rgba(228, 231, 237, 1);
        opacity: 0.5;
        margin-left: 16px;
        margin-top: 5px;
        margin-bottom: 8px;
        margin-right: 16px;
    }

    .firstParameter {
        border: 1px solid #D8DCE6;
        height: 8rem;
        width: 30%;
        margin-right: 1rem;
        margin-left: 1rem;
        float: left;
        font-size: 16px
    }

    .secondParameter {
        border: 1px solid #D8DCE6;
        height: 8rem;
        width: 30%;

        margin-right: 1rem;
        float: left;
        font-size: 16px
    }

    .parameterTop {
        margin-bottom: 1.05rem;
    }

    .parameterImg {
        margin-left: 1rem;
    }

    .parameterEnable {
        margin-right: 1rem;
        float: right;
    }

    .parameterMiddle {
        margin-top: 0.32rem;
    }

    .parameterValue {
        margin-left: 4.5rem;
    }

    .parameterEnableValue {
        margin-right: 1rem;
        float: right;
    }

    .parameterUpLimit {
        margin-top: 1rem;
        margin-left: 2.6rem;
    }

    .parameterUpLimitValue {
        margin-right: 1.7rem;
        float: right;
    }

    .thirdParameter {
        margin-top: 1rem;
        margin-bottom: 2rem;
    }

    .voltageHeight {
        height: 9.5rem;
    }
    .limitInput{
        width: 10rem;
        height: 2rem;
        border-radius:5px;
        border: 1px solid #DBDBDB;
        margin-left: 2rem;
        text-indent:1rem;
    }
    .limitDiv{
        margin-left: 1rem;
        margin-bottom: 1rem;
    }
    .setLimitBtn{
        border-radius:5px;
        border: 1px solid #387EE8;
        width: 6rem;
        height: 2rem;
        font-size: 16px;
        background-color:white;
        cursor: pointer;
        color:#387EE8;
        margin-left: 1rem;
    }

</style>
