<template>
  <div
    style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;"
  >
    <div class="pageBox">
      <div class="left fl">
        <div class="myNavigation">
          <el-breadcrumb separator="/" style="display: inline-block">
            <el-breadcrumb-item>
              <span style="font-size: 16px;color:#303313;"
                >扫码管理/新增二维码</span
              >
            </el-breadcrumb-item>
            <!-- <el-breadcrumb-item>
                        <span style="font-size: 14px;color:#606266;">无线终端列表</span>
          </el-breadcrumb-item>-->
          </el-breadcrumb>
        </div>
<!--        <div class="equipment">-->
<!--          <div class="fl equipmentType">-->
<!--            项目名称-->
<!--            <span style="color:red;">*</span>-->
<!--          </div>-->
<!--          <div class="fl equipmentInput">-->
<!--            <el-select-->
<!--              v-model="projectName"-->
<!--              @change="select(projectName)"-->
<!--              placeholder="请选择"-->
<!--              style="width:100%"-->
<!--            >-->
<!--              <el-option-->
<!--                v-for="item in options"-->
<!--                :key="item.deviceType"-->
<!--                :label="item.typeName"-->
<!--                :value="item.deviceType"-->
<!--              ></el-option>-->
<!--            </el-select>-->
<!--          </div>-->
<!--        </div>-->
<!--        <div class="equipment">-->
<!--          <div class="fl equipmentType">-->
<!--            设备名称-->
<!--            <span style="color:red;">*</span>-->
<!--          </div>-->
<!--          <div class="fl lh equipmentInput">-->
<!--            <el-input-->
<!--              v-model="deviceName"-->
<!--              placeholder="请输入设备名称"-->
<!--            ></el-input>-->
<!--          </div>-->
<!--        </div>-->
<!--        <div class="equipment">-->
<!--          <div class="fl equipmentType">-->
<!--            编号-->
<!--            <span style="color:red;">*</span>-->
<!--          </div>-->
<!--          <div class="fl lh equipmentInput">-->
<!--            <el-input-->
<!--              v-model="siteCode"-->
<!--              placeholder="请输入设备编号"-->
<!--            ></el-input>-->
<!--          </div>-->
<!--        </div>-->
<!--        <div class="equipment">-->
<!--          <div class="fl equipmentType">-->
<!--            设备类型-->
<!--            &lt;!&ndash; <span style="color:red;">*</span> &ndash;&gt;-->
<!--          </div>-->
<!--          <div class="fl equipmentInput">-->
<!--            <el-select-->
<!--              v-model="equipmentType"-->
<!--              @change="selDetector(equipmentType)"-->
<!--              placeholder="请选择"-->
<!--              style="width:100%"-->
<!--            >-->
<!--              <el-option-->
<!--                v-for="item in detectors"-->
<!--                :key="item.detectorType"-->
<!--                :label="item.typeName"-->
<!--                :value="item.detectorType"-->
<!--              ></el-option>-->
<!--            </el-select>-->
<!--          </div>-->
<!--        </div>-->
        <div class="equipment">
          <div class="fl equipmentType">ICCID</div>
          <div class="fl" style="width: 240px;">
            <el-input v-model="number" placeholder="请输入ICCID"></el-input>
          </div>
        </div>
          <div class="equipment">
              <div class="fl equipmentType"> 二维码:</div>
              <div class="fr equipmentInput">
                  <div class="qrCode" ref="hasQrCode" id="qrcode"></div>
                  
              </div>

          </div>
      </div>
<!--      <div class="right fl">-->
<!--        二维码:-->
<!--        <div class="qrCode" ref="hasQrCode" id="qrcode"></div>-->
<!--      </div>-->
      <div class="equipment">
        <div class="fl" style="margin-top:30px;margin-left:125px;">
          <el-button type="primary" @click="save()" :disabled="disabledType">保存</el-button >
          <el-button type="primary" @click="creatQrCode()" style="margin-left: 4rem">生成二维码</el-button>
        </div >
          <div class="fl" style="margin-top:30px;margin-left:125px;">
              <el-button type="primary" @click="goback()">返回</el-button>
              <el-button type="primary" @click="printingQrCode()" style="margin-left: 4rem" >打印二维码</el-button>
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
  name: "AddScanReport",
  data() {
    return {
      routeParam: {},
      projectName: "0",
      options: [
        // {
        //   deviceType: "0",
        //   typeName: "开元市"
        // },
        // {
        //   deviceType: "1",
        //   typeName: "华创园"
        // }
      ],
      detectors: [],
      deviceName: "",
      siteCode: "",
      equipmentType: null,
      deviceTypeName: "",
      number: "",
      siteName: "",
      qrCodeUrl: "",
      disabledType: true
    };
  },
  created: function() {
    this.getEquimentData();
    this.getSiteData();
    this.routeParam = this.$route.params;
    
  },
  methods: {
      goback() {
          this.$router.push('/scanReport');
      },
    getEquimentData() {
      let url = this.detectorUrl;
      let data = {};
      axios.post(url, data).then(res => {
        console.log(res);
        this.equipmentType = res.data.data.detectorList[0].detectorType
        this.detectors = res.data.data.detectorList;
        this.deviceTypeName = this.detectors[0].typeName;
        
        console.log(this.detectors[0].typeName);
      })
      
    },
    getSiteData() {
      let that = this;
      let url = this.getSiteListUrl;
      this.apiPost(url).then(res => {
        let siteList = res.data.getsiteList;
        siteList.map(function(value,index){
          // console.log(value,index)
          that.options.push(
            {
              deviceType: index.toString(),
              typeName: value.siteName
            }
          )
          that.siteName = that.options[0].typeName;
        })
      })
    },
    // 保存二维码
    save() {
      let url = this.saveQRCodeUrl;
      let data = {
        deviceType: this.equipmentType,
        deviceTypeName: this.deviceTypeName,
        deviceCode: this.siteCode,
        siteName: this.siteName,
        deviceName: this.deviceName,
        // deviceCode: "12121",
        deviceSeqCode: this.number,
        qrCodeUrl: this.qrCodeUrl
        // qrCodeUrl: "/fdava/wfa454.html"
      };
      console.log(data);
      axios.post(url, data).then(res => {
        console.log(res.data);
        this.$commonFn.showTip(res.data.message, 1);
      });
    },
    // 生成二维码
    creatQrCode() {
      this.$refs.hasQrCode.innerHTML = "";
      this.qrCodeFn();
    },
    //二维码生成函数
    qrCodeFn() {
      if (this.number == "") {
        this.$commonFn.showTip("请输入ICCID", 3);
        return;
      }
      let that = this;
      let data = {
        code: "1001",
        ICCID: this.number,
        factory: "LYS"
      };
      let qrcode = new QRCode("qrcode", {
        text: JSON.stringify(data),
        width: 150,
        height: 150,
        colorDark: "#000000",
        colorLight: "#ffffff",
        correctLevel: QRCode.CorrectLevel.H
      });
      this.disabledType = false;
      setTimeout(function() {
        that.uplodadImg();
      }, 10);
    },
    uplodadImg() {
      console.log('上传二维码')
      let that = this;
      let el = document.getElementById("qrcode");
      let imgUrl = el.getElementsByTagName("img")[0].src;
      console.log(imgUrl)
      let imgFile = this.base64ToBlob(imgUrl, "png");
      let file = this.blobToFile(imgFile, "qrCpde");
      console.log(imgUrl, file);
      let url = this.uploadQRCode;
      let formData = new FormData();
      formData.append("imgFile", file);
      axios({
        method: "post",
        url: url,
        data: formData,
        headers: {
          "Content-Type": "multipart/form-data"
        }
      }).then(function(res) {
        console.log(res.data);
        that.qrCodeUrl = res.data.data;
      });
    },
    // 打印二维码
    printingQrCode() {
      // this.uplodadImg()
      //判断iframe是否存在，不存在则创建iframe
      let el = document.getElementById("qrcode");
      if (el.innerHTML == "") {
        this.$commonFn.showTip("请先生存二维码", 3);
        return;
      }
      let iframe = document.getElementById("print-iframe");
      if (!iframe) {
        iframe = document.createElement("IFRAME");

        let doc = null;
        iframe.setAttribute("id", "print-iframe");
        // iframe.setAttribute(
        //   "style",
        //   "position:absolute;width:0px;height:0px;left:-400px;top:-500px;"
        // );
        document.body.appendChild(iframe);
        doc = iframe.contentWindow.document;
        console.log("<div>" + el.innerHTML + "</div>");
        //这里可以自定义样式
        //doc.write("<LINK rel="stylesheet" type="text/css" href="css/print.css">");
        doc.write("<div style='width:150px;height:150px;transform: rotate(-90deg);'>" + "<div style='width:10%;'>" + el.innerHTML + "</div>" + "<div style='width:150px;font-size:13.5px;text-align: center;'>" + this.number + "</div>" + "</div>");
        doc.close();
        iframe.contentWindow.focus();
      }
      iframe.contentWindow.print();
      document.body.removeChild(iframe);
      if (navigator.userAgent.indexOf("MSIE") > 0) {
        document.body.removeChild(iframe);
      }
      // this.PageSetup_del();
    },
    
    printingQrCode2() {
      let el = document.getElementById("qrcode");
      if (el.innerHTML == "") {
        this.$commonFn.showTip("请先生成二维码", 3);
        return;
      }
      this.CreateOneFormPage();
      // LODOP.PRINT(); //直接打印
      // LODOP.PREVIEW();  //预览打印
      LODOP.PRINT_DESIGN(); //打印设计
    },
    CreateOneFormPage() {
        let data = {
          code: "1001",
          ICCID: this.number,
          factory: "LYS"
        };
        LODOP = getLodop();
        // LODOP.PRINT_INIT("订货单");
        // LODOP.SET_PRINT_STYLE("FontSize", 18);
        // LODOP.SET_PRINT_STYLE("Bold", 1);
        // // LODOP.ADD_PRINT_TEXT(50, 231, 260, 39, "打印页面部分内容");
        // LODOP.ADD_PRINT_HTM(0, 0, 220, 220, document.getElementById("qrcode").innerHTML);  
        // LODOP.SET_PRINT_PAGESIZE(1,400000,300000)
        LODOP.ADD_PRINT_BARCODE(0,0,'4cm','3cm','QRCode',JSON.stringify(data)); //打印二维码 

      },
    base64ToBlob(urlData, type) {
      let arr = urlData.split(",");
      console.log(arr)
      let mime = arr[0].match(/:(.*?);/)[1] || type;
      // 去掉url的头，并转化为byte
      let bytes = window.atob(arr[1]);
      // 处理异常,将ascii码小于0的转换为大于0
      let ab = new ArrayBuffer(bytes.length);
      // 生成视图（直接针对内存）：8位无符号整数，长度1个字节
      let ia = new Uint8Array(ab);
      for (let i = 0; i < bytes.length; i++) {
        ia[i] = bytes.charCodeAt(i);
      }
      return new Blob([ab], {
        type: mime
      });
    },
    //将blob转换为file
    blobToFile(theBlob, fileName) {
      theBlob.lastModifiedDate = new Date();
      theBlob.name = fileName;
      return theBlob;
    },

    select(num) {
      console.log(num);
      this.siteName = this.options[num].typeName;
    },
    selDetector(num) {
      this.deviceTypeName = this.detectors[num].typeName;
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
/* .pageBox{
    display: flex;
    justify-content: space-between;
} */
.left {
  width: 500px;
}
.right {
  /* width: 500px; */
  margin-top: 40px;
}
.qrCode {
  width: 170px;
  height: 170px;
  padding: 20px 0px 0px 20px;
  border: 1px solid #ccc;
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
  margin: 5px 16px 0 16px;
}
.equipmentType {
  width: 92px;
  line-height: 32px;
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
</style>
<style media="print">
@page {
  margin-bottom: 0mm;
  margin-top: 0mm
}
</style>
