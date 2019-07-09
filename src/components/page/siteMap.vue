<template>
    <div>
        <div style="width:100%; position: absolute; z-index: 10;">
            <el-button class="v-1-0-0-Button" @click="backToControl"
                       style="position: absolute; margin-left: 1%; margin-top: 1.5%;">
                <i class="el-icon-back"> 返回系统</i>
            </el-button>
            <!-- <img src="../../resource/img/big-bg-header.png" width="100%" height="100%"> -->
        </div>
        <!--logo-->
        <div style="width:100%;height:75px;background:#065D8A">
            <div style="width:26.5%; height:8%; position: absolute; z-index: 11; left: 36.7%; top: 0.4%; ">
                <div style="width:30px; height:35px; position: absolute; z-index: 12;top:10%;
            float: left;">
                    <img src="../../resource/img/logo5.png" width="100%" height="100%">
                </div>

                <P style="position: absolute;line-height: 60px;letter-spacing:2px;font-size: 1.5rem;
                margin-left:12.5%;margin-top: -2px; color: white;">
                    电气火灾智能监控管理平台
                </P>
                <!-- <img src="../../assets/img/didian.gif" alt=""> -->
            </div>
        </div>
        
        <div id="main_map" class='main_map' ref="mainMap" style="top:8%;"></div>
        <!-- <div ref="mainMap"></div> -->
    </div>
</template>

<script>
    import http from '../../common/http';

    export default {
        name: "SiteMap",

        data() {
            return {
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
                mqttData:[],
                mqttList:[],
                mqttDataMarker:[],
                mqttListMarker:[],
                infoBox:'',
                sessionCode: '',
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
            this.getMapData();
        },
        methods: {
            backToControl() {
                this.$router.push('/main');
            },
            //生成地图
            createMap() {
                this.map = new BMap.Map(this.$refs.mainMap); //创建Map实例
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
                    styleJson: [this.styleJson]
                })
                let that = this;
                this.map.addEventListener("zoomend",function (e) {
                    let zoomNum = that.map.getZoom();
                })
            },
            
            getMapData(type){
                let url = this.getSiteListUrl;
                let data = {};
                this.apiPost(url, data).then((res) => {
                    if (res.status === 200) {
                        this.mqttData = res.data.getsiteList;
                           this.createPoint(this.mqttData);
                    }
                    else {
                        this.$commonFn.showTip(res.message, 3);
                    }
                }, (err) => {
                });
            },
            
            createPoint(list) {
                //初始化
                this.createMap();
                let that = this;
                //设置标注的图标
                this.icon_normal1 = new BMap.Icon('http://easy913.com/m/webPdf/img/siteRed.gif', new BMap.Size(40, 40));
                this.icon_alarm = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/map_icon_location_red.png', new BMap.Size(40, 40));
                //this.icon_normal1 = new BMap.Icon('https://lys-arc.oss-cn-hangzhou.aliyuncs.com/siteRed.gif', new BMap.Size(40, 40));
                console.log(list)
                //设置标注的经纬度
                for (let i = 0; i < list.length; i++) {
                    let lng = list[i].longitute;
                    let lat = list[i].latitude;
                    if( this.sessionCode != null){
                        if(this.sessionCode == list[i].siteCord){
                            this.map.centerAndZoom(new BMap.Point(list[i].longitute, list[i].latitude), list[i].mapNum);
                        }

                    }else{
                        this.map.centerAndZoom(new BMap.Point(lng, lat), list[i].mapNum);
                    }
                    let efmMarker;
                    efmMarker = new BMap.Marker(new BMap.Point(lng, lat), {icon: this.icon_normal1});
                    this.map.addOverlay(efmMarker);
                    efmMarker.addEventListener("click", function () {

                        console.log("dianjishijian")
                        that.$router.push(
                            {
                                name: 'projectDetail'
                            }
                        );
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
        },
        mounted() {
            //this.createMap();
        },
        mixins: [http]
    }
</script>

<style scoped>

</style>
