<template>
    <div>
        <div v-if="showMap" id="main_map" class='main_map' ref="mainMap"></div>
        <div v-else id="main_device_map" class='main_map' ref="mainDeviceMap"></div>
    </div>
</template>

<script>
    import http from '../../common/http';

    export default {
        name: "SiteDetailMap",

        data() {
            return {
                showMap: true,
                // userID: 'admin_',
                websocket: null,
                postValue: {},
                // tagsList: [],
                map: {},
                mapPoint: {},
                mapMarker: {},
                canvas: {},
                collapse: false,
                isShow: false,
                // isTable: true,
                loc_Icon: {},
                loc_marker: {},
                loc_Con: {},
                loc_infowindow: {},
                loc_Icon1: {},
                loc_marker1: {},
                efmList:[],
                mqttSiteData:[],
                mqttData:[],
                mqttList:[],
                mqttDataMarker:[],
                mqttListMarker:[],
                infoBox:'',
                sessionCode: '',
                centerPoint: '',
                styleJson: [
                    {
                        "featureType": "road",
                        "elementType": "all",
                        "stylers": {
                            "lightness": 20
                        }
                    }, {
                        "featureType": "highway",
                        "elementType": "geometry",
                        "stylers": {
                            "color": "#f49935",
                            "visibility": "on"
                        }
                    }, {
                        "featureType": "railway",
                        "elementType": "all",
                        "stylers": {
                            "visibility": "off"
                        }
                    }, {
                        "featureType": "local",
                        "elementType": "labels",
                        "stylers": {
                            "visibility": "off"
                        }
                    }, {
                        "featureType": "water",
                        "elementType": "all",
                        "stylers": {
                            "color": "#d1e5ff"
                        }
                    }, {
                        "featureType": "poi",
                        "elementType": "labels",
                        "stylers": {
                            "visibility": "off"
                        }
                    }, {
                        "featureType": "green",
                        "elementType": "geometry",
                        "stylers": {
                            "color": "#c3eb9bff"
                        }
                    }, {
                        "featureType": "water",
                        "elementType": "geometry",
                        "stylers": {
                            "color": "#066f91ff"
                        }
                    }, {
                        "featureType": "building",
                        "elementType": "geometry.fill",
                        "stylers": {
                            "color": "#daf5eeff"
                        }
                    }, {
                        "featureType": "building",
                        "elementType": "geometry.stroke",
                        "stylers": {
                            "color": "#4a90e2ff"
                        }
                    }, {
                        "featureType": "nationalway",
                        "elementType": "geometry",
                        "stylers": {
                            "visibility": "off"
                        }
                    }, {
                        "featureType": "arterial",
                        "elementType": "geometry",
                        "stylers": {
                            "visibility": "on"
                        }
                    }, {
                        "featureType": "tertiaryway",
                        "elementType": "geometry",
                        "stylers": {
                            "visibility": "on"
                        }
                    }, {
                        "featureType": "tertiaryway",
                        "elementType": "geometry.fill",
                        "stylers": {
                            "color": "#faa005ff"
                        }
                    }, {
                        "featureType": "fourlevelway",
                        "elementType": "geometry",
                        "stylers": {
                            "visibility": "on"
                        }
                    }, {
                        "featureType": "fourlevelway",
                        "elementType": "geometry.fill",
                        "stylers": {
                            "color": "#f79e09ff"
                        }
                    }, {
                        "featureType": "local",
                        "elementType": "geometry.fill",
                        "stylers": {
                            "color": "#aca9a9ff"
                        }
                    }, {
                        "featureType": "district",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "weight": 40
                        }
                    }, {
                        "featureType": "manmade",
                        "elementType": "geometry",
                        "stylers": {
                            "color": "#f5e5caff"
                        }
                    }, {
                        "featureType": "estatelabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#b8b1b1ff"
                        }
                    }, {
                        "featureType": "businesstowerlabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#948b8bff"
                        }
                    }, {
                        "featureType": "governmentlabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#db7070ff"
                        }
                    }, {
                        "featureType": "restaurantlabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#a39d9dff"
                        }
                    }, {
                        "featureType": "hotellabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#999393ff"
                        }
                    }, {
                        "featureType": "shoppinglabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#918c8cff"
                        }
                    }, {
                        "featureType": "lifeservicelabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#9c9797ff"
                        }
                    }, {
                        "featureType": "carservicelabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#8f8787ff"
                        }
                    }, {
                        "featureType": "transportationlabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#8f8a8aff"
                        }
                    }, {
                        "featureType": "financelabel",
                        "elementType": "labels.text.fill",
                        "stylers": {
                            "color": "#5f5b5bff"
                        }
                    }
                ]
                
            }
        },
         created: function () {
            //获取地图和中控信息
            // this.getMapInfo();
            
            this.sessionCode = sessionStorage.getItem('siteCode')
            console.log(this.sessionCode)
            this.getMapSiteData();
        },
        methods: {
            backToControl() {
                this.$router.push('/main');
            },
            //生成项目地图
            createSiteMap() {
                this.siteMap = new BMap.Map(this.$refs.mainMap); //创建Map实例
                this.siteMap.addControl(new BMap.MapTypeControl({//添加地图类型控件
                    mapTypes: [
                        BMAP_NORMAL_MAP,
                        BMAP_HYBRID_MAP
                    ]
                }));
                this.siteMap.addControl(new BMap.NavigationControl({type: BMAP_ANCHOR_TOP_RIGHT}));
                //this.siteMap.setCurrentCity("北京");              //设置地图显示的城市 此项是必须设置的
                this.siteMap.enableScrollWheelZoom(true);        //开启鼠标滚轮缩放

                //设置地图样式
                this.siteMap.setMapStyle({
                    styleJson: [this.styleJson]
                })
                let that = this;
                this.siteMap.addEventListener("zoomend",function (e) {
                    let zoomNum = that.siteMap.getZoom();
                })
            },
            getMapSiteData(type){
                let url = this.getSiteListUrl;
                let data = {};
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.mqttSiteData = res.data.getsiteList;
                           this.createSitePoint(res.data);
                    }
                    else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {
                });
            },
            
            createSitePoint(data) {
                //初始化
                this.createSiteMap();
                let that = this;
                let list = data.getsiteList
                //设置标注的图标
                this.icon_normal1 = new BMap.Icon('http://easy913.com/m/webPdf/img/siteRed.gif', new BMap.Size(40, 40));
                this.icon_alarm = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location_red.png', new BMap.Size(40, 40));
                console.log(data.centerJ, data.centerW, data.mapNum)
                console.log(list)
                //设置标注的经纬度
                for (let i = 0; i < list.length; i++) {
                    let lng = list[i].longitute;
                    let lat = list[i].latitude;
                    // if( this.sessionCode != null){
                    //     if(this.sessionCode == list[i].siteCord){
                    //         this.siteMap.centerAndZoom(new BMap.Point(list[i].longitute, list[i].latitude), list[i].mapNum);
                    //     }

                    // }else{
                    //     this.siteMap.centerAndZoom(new BMap.Point(data.centerJ, data.centerW), data.mapNum);
                    // }
                    this.siteMap.centerAndZoom(new BMap.Point(data.centerJ, data.centerW), data.mapNum);
                    let efmMarkerSite;
                    efmMarkerSite = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_normal1});
                    this.siteMap.addOverlay(efmMarkerSite);
                    efmMarkerSite.addEventListener("click", function () {
                        console.log('dianjia')
                        that.getCenterData(list[i].siteCord)
                        that.getMapInfo(list[i].siteCord);
                        that.showMap = false;
                        // that.$router.push(
                        //     {
                        //         path: '/mapDetail'
                        //     }
                        // );
                    });
                    // efmMarker.addEventListener('mousedown',function(){
                    //     if(event.button == 0){
                    //         console.log('鼠标左键')
                    //         that.$router.push(
                    //             {
                    //                 path: '/mapDetail'
                                    
                    //             }
                    //         )
                    //     }else if(event.button == 2){
                    //         console.log('鼠标右键')
                    //         that.$router.push(
                    //             {
                    //                 path: '/mapDetail'
                                    
                    //             }
                    //         )
                    //     }
                    // }, false)
                }
            },
            getCenterData(code) {
                let url = this.centerPointUrl;
                let data = {
                    siteCode: code
                }
                this.apiPost(url, data).then((res) => {
                    console.log(res.data)
                    this.centerPoint = res.data
                })
            },
            //设备地图开始
            //生成地图
            createMap() {
                this.map = new BMap.Map(this.$refs.mainDeviceMap); //创建Map实例
                this.map.addControl(new BMap.MapTypeControl({//添加地图类型控件
                    mapTypes: [
                        BMAP_NORMAL_MAP,
                        BMAP_HYBRID_MAP
                    ]
                }));
                this.map.addControl(new BMap.NavigationControl({type: BMAP_ANCHOR_TOP_RIGHT}));
                //this.map.setCurrentCity("北京");              //设置地图显示的城市 此项是必须设置的
                this.map.enableScrollWheelZoom(true);        //开启鼠标滚轮缩放

                //设置地图样式
                this.map.setMapStyle({
                    styleJson: this.styleJson
                })
                let that = this;
                this.map.addEventListener("zoomend",function (e) {
                    let zoomNum = that.map.getZoom();
                    console.log(zoomNum)
                    if (zoomNum >= 17){
                        for (let i = 0;i < that.mqttDataMarker.length;i++){
                            that.map.removeOverlay(that.mqttDataMarker[i]);
                        }
                        for (let i = 0;i < that.mqttListMarker.length;i++){
                            that.map.removeOverlay(that.mqttListMarker[i]);
                        }
                        that.createMqttEquip(that.mqttList);
                    }
                    else{
                        for (let i = 0;i < that.mqttDataMarker.length;i++){
                            that.map.removeOverlay(that.mqttDataMarker[i]);
                        }
                        for (let i = 0;i < that.mqttListMarker.length;i++){
                            that.map.removeOverlay(that.mqttListMarker[i]);
                        }
                        that.createMqttPoint(that.mqttData);
                    }
                })
            },
            createMqttEquip(list){
                this.mqttListMarker = [];
                //设置标注的图标
                this.icon_normal = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location.png', new BMap.Size(40, 40));
                this.icon_alarm = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location_red.png', new BMap.Size(40, 40));
                //设置标注的经纬度
                for (let i = 0; i < list.length; i++) {
                    let lng = list[i].lngBmap;
                    let lat = list[i].latBmap;
                    // if(lng && lat){
                    //     this.map.centerAndZoom(new BMap.Point(lng, lat), 18);
                    // }
                    let efmMarker;
                    if(list[i].warnNo){
                        efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_alarm});
                    }
                    else{
                        efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_normal});
                    }
                    this.mqttListMarker.push(efmMarker);
                    this.map.addOverlay(efmMarker);
                }
            },
            getMapInfo(siteCode) {
                // console.log('quguan11')
                let _loading = this.$commonFn.showLoading(2, '.main-box');
                let url = this.getMapInfoUrl;
                let data = {
                    siteCode:siteCode,
                    // 'siteCode':sessionStorage.getItem('siteCode')
                };
                this.apiPost(url, data).then((res) => {
                    _loading.close();
                    if (res.status === 200) {
                         //mapSign为0代表显示默认地图，为1代表显示项目地图
                        console.info("res.data.mapSign",res.data.mapSign);
                        if(res.data.mapSign==0) {
                            let efmList = res.data.getDeviceList;
                            this.createPoint(efmList);
                            //this.getMapData(siteCode);
                            this.getMapWireless(siteCode);
                        }else {
                            this.createDevicePoint(res);

                        }
					}
                    else {
                        this.$commonFn.showTip(res.message, 3);
                    }
 
                }, (err) => {
                    _loading.close();
                });
            },
            getMapData(siteCode){
                let url = this.mapDataUrl;
                let data = {
                    siteCode:siteCode,
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.mqttData = res.data.mapData;

                           // this.createPoint(this.mqttData);


                    }
                    else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {
                });
            },
            getMapWireless(siteCode){
                let url = this.mapWirelessUrl;
                let data = {
                    siteCode:siteCode,
                };
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.mqttList = res.data.wirelessEquiplist;
                        this.createMqttPoint(this.mqttList);
                    }
                    else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {
                });
            },
            createMqttPoint(list) {
                let that = this;
                let typeArr = ['电弧探测器','组合式探测器','测温式探测器'];
                this.mqttDataMarker = [];
                //设置标注的图标
                this.icon_normal = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location.png', new BMap.Size(40, 40));
                this.icon_alarm = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location_red.png', new BMap.Size(40, 40));
                let boxStyle = {
                    boxStyle: {
                        background: 'rgb(56,125,299)',
                        width: "200px",
                        height: '100px',
                        borderRadius: '5px',
                        marginBottom: '20px'
                    },
                    closeIconMargin: "10px 10px 0 0",
                    enableAutoPan: true,
                    closeIconUrl: "https://lys-arc.oss-cn-hangzhou.aliyuncs.com/close.png",
                    alignBottom: false
                };
                let infoBox = '';
                //设置标注的经纬度
                console.log(list);
                if(!list){
                    return;
                }
                for (let i = 0; i < list.length; i++) {
                    let lng = list[i].lngBmap;
                    let lat = list[i].latBmap;
                    // if(lng && lat){
                    //     this.map.centerAndZoom(new BMap.Point(lng, lat), 14);
                    // }
                    let efmMarker;
                      if(!list[i].warnNo){
                          efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_normal});
                      }else{
                          efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_alarm});
                      }


                    // this.mqttDataMarker.push(efmMarker);
                    // this.map.addOverlay(efmMarker);
                    // efmMarker.addEventListener("mouseover", function () {
                    //     if(list[i].siteName){
                    //         list[i].siteLabel = list[i].siteName;
                    //     }
                    //     else{
                    //         list[i].siteLabel = list[i].siteCode;
                    //     }
                    //     let loc_Con = '<div style="color: #fff;margin: 12px 16px;font-size: 14px;"><h4 style="margin-bottom: 8px">'+list[i].siteLabel+'无线</h4>' +
                    //         '<div style="color: #b9d2f6"><span style="display: inline-block;margin-bottom: 2px">设备数</span><span style="float: right;display: inline-block;padding-top: 2px;color:#fff;font-weight: bold">'+list[i].countData.equipCount+'</span><br/>' +
                    //         '<span style="display: inline-block;margin-bottom: 2px">报警数</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+list[i].countData.alarmCount+'</span><br/>' +
                    //         '<span style="display: inline-block;margin-bottom: 2px">故障数</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+list[i].countData.faultCount+'</span><br/>' +
                    //         '<span style="display: inline-block;margin-bottom: 2px">离线数</span><span style="float: right;font-weight: bold">'+list[i].countData.offlineCount+'</span></div></div>';
                    //     if (infoBox){
                    //         infoBox.close();
                    //     }
                    //     infoBox = new BMapLib.InfoBox(this.map, loc_Con, boxStyle);
                    //     infoBox.open(efmMarker);
                    this.map.addOverlay(efmMarker); // 将标注添加到地图中
                    efmMarker.addEventListener("mouseover", function () {
                        let efmLabel ='无线终端';
                        console.log(list[i],i);
                        let serialNumber=parseInt(list[i].lineCode) + 1 + "-" + parseInt(list[i].addr,10);
                        let type = typeArr[list[i].typesssss];

                        let loc_Con ='<div style="color: #fff;margin: 12px 16px;font-size: 14px;"><div style="color: #b9d2f6"><span style="display: inline-block;margin-bottom: 2px">设备名称：</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+efmLabel+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">设备编号：</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+serialNumber+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">设备类型：</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+type+'</span></div></div>';

                        if (infoBox){
                            infoBox.close();
                        }
                        infoBox = new BMapLib.InfoBox(this.map, loc_Con, boxStyle);
                        infoBox.open(efmMarker);
                    });
                    efmMarker.addEventListener('click', function() {
                        // that.getMapSiteData();
                        // that.showMap = true;
                        that.$router.push(
                            {
                                name: 'projectDetail'
                                
                            }
                        )
                    })
                }
            },
            createPoint(list) {
                let that = this;
                //初始化
                this.createMap();
                //设置标注的图标
                this.icon_normal = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location.png', new BMap.Size(40, 40));
                this.icon_alarm = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location_red.png', new BMap.Size(40, 40));
                let boxStyle = {
                    boxStyle: {
                        background: 'rgb(56,125,299)',
                        width: "140px",
                        height: '135px',
                        borderRadius: '5px',
                        marginBottom: '20px'
                    },
                    closeIconMargin: "10px 10px 0 0",
                    enableAutoPan: true,
                    closeIconUrl: "https://lys-arc.oss-cn-hangzhou.aliyuncs.com/close.png",
                    alignBottom: false
                };
                let infoBox = '';
                //设置标注的经纬度
                console.log(this.centerPoint)
                this.map.centerAndZoom(new BMap.Point(this.centerPoint.centerJ, this.centerPoint.centerW), this.centerPoint.mapNum);
                for (let i = 0; i < list.length; i++) {
                    let lng = list[i].longitute;
                    let lat = list[i].latitude;
                    // if(lng && lat){
                    //     this.map.centerAndZoom(new BMap.Point(lng, lat), 14);
                    // }
                    let efmMarker;
                    if(list[i].equipAlarmCount > 0){
                        efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_alarm});
                    }
                    else{
                        efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_normal});
                    }
                    this.map.addOverlay(efmMarker);
                    efmMarker.addEventListener("mouseover", function () {
                        if(list[i].niName){
                            list[i].efmLabel = list[i].niName;
                        }
                        else if(list[i].specificator){
                            list[i].efmLabel = list[i].specificator;
                        }
                        else{
                            list[i].efmLabel = parseInt(list[i].device_code,16);
                        }
                        let loc_Con = '<div style="color: #fff;margin: 12px 16px;font-size: 14px;"><h4 style="margin-bottom: 8px">'+list[i].efmLabel+'</h4>' +
                            '<div style="color: #b9d2f6"><span style="display: inline-block;margin-bottom: 2px">设备数</span><span style="float: right;display: inline-block;padding-top: 2px;color:#fff;font-weight: bold">'+list[i].equipCount+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">报警数</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+list[i].equipAlarmCount+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">故障数</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+list[i].equipFaultCount+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">离线数</span><span style="float: right;font-weight: bold">'+list[i].equipOffLineCount+'</span></div></div>';
                        if (infoBox){
                            infoBox.close();
                        }
                        infoBox = new BMapLib.InfoBox(this.map, loc_Con, boxStyle);
                        infoBox.open(efmMarker);
                    });
                    efmMarker.addEventListener('click', function() {
                        // that.getMapSiteData();
                        // that.showMap = true;
                        that.$router.push(
                            {
                                name: 'projectDetail'
                                
                            }
                        )
                    })
                }
            },
            //区管账号地图
            createDevicePoint(res) {
                let that = this;
                //设置标注的图标
                this.icon_normal = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location.png', new BMap.Size(40, 40));
                //this.icon_normal = new BMap.Icon('http://www.toysoho.com/download/img/map_icon_location.png', new BMap.Size(40, 40));
                this.icon_alarm = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location_red.png', new BMap.Size(40, 40));
                let boxStyle = {
                    boxStyle: {
                        background: 'rgb(56,125,299)',
                        width: "200px",
                        height: '100px',
                        borderRadius: '5px',
                        marginBottom: '20px'
                    },
                    closeIconMargin: "10px 10px 0 0",
                    enableAutoPan: true,
                    //closeIconUrl: "http://www.toysoho.com/download/img/close.png",
                    closeIconUrl: "https://lys-arc.oss-cn-hangzhou.aliyuncs.com/close.png",
                    alignBottom: false
                };
                let latitude=res.data.latitude;
                let longitute=res.data.longitute;
                let mapNum=res.data.mapNum;
                // 百度地图API功能
                this.map = new BMap.Map(this.$refs.mainDeviceMap,{enableMapClick:false}); //创建Map实例
                // 百度地图API功能
                //let top_left_navigation = new BMap.NavigationControl();  //左上角，添加默认缩放平移控件
                let top_left_control = new BMap.ScaleControl({anchor: BMAP_ANCHOR_TOP_LEFT});// 左上角，添加比例尺
                this.map.addControl(top_left_control);
                //this.map.addControl(top_left_navigation);
                console.log("地图级别:"+mapNum);
                // this.map.centerAndZoom(new BMap.Point(longitute,latitude),mapNum);  // 初始化地图,设置中心点坐标和地图级别
                this.map.centerAndZoom(new BMap.Point(this.centerPoint.centerJ, this.centerPoint.centerW), this.centerPoint.mapNum);
                // this.map.enableScrollWheelZoom(true); // 开启鼠标滚轮缩放
                this.map.clearOverlays();
                this.map.disableDragging();//禁用拖动
                // 创建室内图实例
                let indoorManager = new BMapLib.IndoorManager(this.map);
                //设置标注的经纬度
                let list=res.data.getDeviceList;
                let infoBox = '';
                let typeArr = ['电弧探测器','组合式探测器','测温式探测器'];
                for (let i = 0; i < list.length; i++) {
                    let lng = list[i].lngBmap;
                    let lat = list[i].latBmap;
                    let efmMarker ;  // 创建标注
                    if(list[i].warningNo==null || list[i].warningNo.length==0){
                        efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_normal});
                    }
                    else{

                        efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_alarm});
                    }
                    this.map.addOverlay(efmMarker); // 将标注添加到地图中
                    efmMarker.addEventListener("mouseover", function () {
                        let efmLabel ='无线终端';
                        let serialNumber=parseInt(list[i].line_code) + 1 + "-" + parseInt(list[i].addr,10);
                        let type = typeArr[list[i].type];

                        let loc_Con ='<div style="color: #fff;margin: 12px 16px;font-size: 14px;"><div style="color: #b9d2f6"><span style="display: inline-block;margin-bottom: 2px">设备名称：</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+efmLabel+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">设备编号：</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+serialNumber+'</span><br/>' +
                            '<span style="display: inline-block;margin-bottom: 2px">设备类型：</span><span style="color: #ffa0a0;float: right;font-weight: bold">'+type+'</span></div></div>';
                        if (infoBox){
                            infoBox.close();
                        }
                        infoBox = new BMapLib.InfoBox(this.map, loc_Con, boxStyle);
                        infoBox.open(efmMarker);
                    });
                    efmMarker.addEventListener('click', function() {
                        // that.getMapSiteData();
                        // that.showMap = true;
                        that.$router.push(
                            {
                                name: 'projectDetail'
                                
                            }
                        )
                    })
                }
                // indoorManager.hideBaseMap();
            }
        },
        mounted() {
            //this.createMap();
        },
        mixins: [http]
    }
</script>

<style scoped>

</style>
