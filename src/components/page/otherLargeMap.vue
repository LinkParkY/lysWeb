<template>
    <div style=" width:100%; height:100%; background-color: white; overflow: auto; ">
        <div id="container" style=" width:100%; height:100%; background-color: white; overflow: auto; ">

        </div>
    </div>
</template>

<script>
    import http from '../../common/http';
    export default {
        name: "otherLargeMap",
        data() {
            return {

            }
        },
        created: function() {
            // this.loadMap()
        },
        mounted() {
            this.loadMap()
        },
        methods: {
            loadMap() {
            let map = new BMap.Map("container");          // 创建地图实例  
            let point = new BMap.Point(119.33022111, 26.0471255); //地图的中心点
                map.centerAndZoom(point, 11);  // 初始化地图,设置中心点坐标和地图级别
                //添加地图类型控件
                // map.addControl(new BMap.MapTypeControl({
                //     mapTypes:[
                //         BMAP_NORMAL_MAP,
                //         BMAP_HYBRID_MAP
                //     ]}));	  
                // map.setCurrentCity("北京");          // 设置地图显示的城市 此项是必须设置的
                map.enableScrollWheelZoom(true);        //开启鼠标滚轮缩放
            let marker = new BMap.Marker(point);        //创建标注 
                map.addOverlay(marker);                 // 将标注添加到地图中 
            }
        }
    }
</script>