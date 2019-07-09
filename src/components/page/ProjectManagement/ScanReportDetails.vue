<template>
  <div style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">
    <div class="pageBox">
      <div class="left fl">
        <div class="myNavigation">
          <el-breadcrumb separator="/" style="display: inline-block">
            <el-breadcrumb-item>
              <span style="font-size: 16px;color:#303313;">扫码管理/查看二维码</span>
            </el-breadcrumb-item>
            <!-- <el-breadcrumb-item>
                        <span style="font-size: 14px;color:#606266;">无线终端列表</span>
            </el-breadcrumb-item>-->
          </el-breadcrumb>
        </div>
        <div class="equipment">
          <div class="fl equipmentType">项目名称</div>
          <div class="fl equipmentInput">{{siteName}}</div>
        </div>

        <div class="equipment">
          <div class="fl equipmentType">编号</div>
          <div class="fl equipmentInput">{{parseInt(deviceCode, 16) + '-' + (parseInt(lineCode) + 1) + '-' + addr}}</div>
        </div>
        <div class="equipment">
          <div class="fl equipmentType">设备类型</div>
          <div class="fl equipmentInput"><span v-if="deviceType == 0">电弧探测器</span>
              <span v-if="deviceType == 1">组合式探测器</span></div>
        </div>
        <div class="equipment">
          <div class="fl equipmentType">ICCID:</div>
          <div class="fl equipmentInput">{{ICCID}}</div>
        </div>
      </div>
      <div class="right fl">
        二维码:
        <div class="qrCodeStyle" id="qrcode">
          <img :src="url" alt>
        </div>
      </div>
      <div class="equipment">
        <div class="fl" style="margin-top:30px;margin-left:120px;">
          <el-button type="primary" @click="printingQrCode()">打印二维码</el-button>
          <el-button type="primary" @click="goback()">返回</el-button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import http from "../../../common/http.js";
import bus from "../../../common/bus";
import axios from "axios";
export default {
  name: "ScanReportDetail",
  data() {
    return {
      routeParams: {},
      imgUrl: '',
      siteName: '',
      deviceName: '',
      deviceCode: '',
      deviceTypeName: '',
      deviceSeqCode: '',
        ICCID:'',
        lineCode:'',
        addr:'',
        deviceCode:'',
        url:'',
        deviceType: 0,
    };
  },
  created: function() {
    this.routeParams = this.$route.params;
    this.ICCID=this.routeParams.ICCID;
     this.lineCode=this.routeParams.lineCode;
        this.addr=this.routeParams.addr;
        this.deviceCode=this.routeParams.deviceCode;
        this.url=this.routeParams.url;
        this.siteName=this.routeParams.siteName;
      this.deviceType=this.routeParams.deviceType;
      // this.getQRCode();
  },
  mounted() {
    
    this.ICCID=this.routeParams.ICCID;
    this.lineCode=this.routeParams.lineCode;
    this.addr=this.routeParams.addr;
    this.deviceCode=this.routeParams.deviceCode;
    this.url=this.routeParams.url;
    this.siteName=this.routeParams.siteName;
    this.deviceType=this.routeParams.deviceType;
    console.log(this.url)
  },
  methods: {
    goback() {
      this.$router.push('/scanReport');
    },
    getQRCode() {
      let url = this.queryQRCode;
      let data = {
        deviceSeqCode: this.routeParams.deviceSeqCode
        // deviceSeqCode: '1914-020201-01-000075'
      };
      axios.get(url, {
          params: data
        })
        .then(res => {
          this.imgUrl = res.data.data.equipQRCode.qrCodeUrl
          this.siteName = res.data.data.equipQRCode.siteName
          this.deviceName = res.data.data.equipQRCode.deviceName
          this.deviceCode = res.data.data.equipQRCode.deviceCode
          this.deviceTypeName = res.data.data.equipQRCode.deviceTypeName
          this.deviceSeqCode = res.data.data.equipQRCode.deviceSeqCode
          console.log(res.data);
        });
    },
    // 打印二维码
    printingQrCode() {
      // this.uplodadImg()
      //判断iframe是否存在，不存在则创建iframe
      console.log(this.url)
      let iframe = document.getElementById("print-iframe");
      if(!iframe){
        let el = document.getElementById("qrcode");
        console.log(el)
        iframe = document.createElement("IFRAME");
        
        let doc = null;
        iframe.setAttribute("id", "print-iframe");
        // iframe.setAttribute(
        //   "style",
        //   "position:absolute;width:0px;height:0px;left:-400px;top:-500px;"
        // );
        document.body.appendChild(iframe);
        doc = iframe.contentWindow.document;
        console.log("<div>" + el.innerHTML + "</div>")
        //这里可以自定义样式
        //doc.write("<LINK rel="stylesheet" type="text/css" href="css/print.css">");
        // doc.write("<div style='width:10%;'>" + el.innerHTML + "</div>");
        // doc.write("<div style='width:10%;'>" + el.innerHTML + "</div>");
        doc.write("<div style='width:150px;height:150px;transform: rotate(-90deg);'>" + "<div style='width:10%;'>" + el.innerHTML + "</div>" + "<div style='width:150px;font-size:13.5px;text-align: center;'>" + this.ICCID + "</div>" + "</div>");
        doc.close();
        iframe.contentWindow.focus();
      }
      iframe.contentWindow.print();
      document.body.removeChild(iframe);
      if (navigator.userAgent.indexOf("MSIE") > 0) {
        document.body.removeChild(iframe);
      }
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
.left {
  width: 500px;
}
.right {
  /* width: 500px; */
  margin-top: 40px;
}
.fl {
  float: left;
}
.fr {
  float: right;
}
.equipment {
  width: 415px;
  height: 50px;
  line-height: 50px;
  margin: 5px 16px 0 16px;
}
.equipmentType {
  width: 92px;
  /* line-height: 32px; */
  margin-right: 20px;
}
.equipmentInput {
  width: 300px;
}
.lh {
  line-height: 32px;
}
.showEquipment {
  width: 60px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.dialogBtn {
  padding: 0 100px;
  display: flex;
  justify-content: space-between;
}
.el-dialog__body {
  height: 400px !important;
  overflow: auto !important;
}
.oldFile {
  width: 64px;
  margin-right: 10px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.qrCodeStyle {
  width: 150px;
  height: 150px;
  padding: 10px;
  border: 1px solid #ccc;
}
.qrCodeStyle > img {
  width: 100%;
}
</style>
