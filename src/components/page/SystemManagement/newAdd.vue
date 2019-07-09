<template>
  <div style=" width:100%; height:100%; background-color: #F0F2F5; overflow:auto;">
    <div class="pageBox">
      <div class="myNavigation">
        <el-breadcrumb separator="/" style="display: inline-block">
          <el-breadcrumb-item>
            <span v-if="routeParam.type" style="font-size: 16px;color:#303313;">修改升级记录</span>
            <span v-else style="font-size: 16px;color:#303313;">新增升级记录</span>
          </el-breadcrumb-item>
          <!-- <el-breadcrumb-item>
                        <span style="font-size: 14px;color:#606266;">无线终端列表</span>
          </el-breadcrumb-item>-->
        </el-breadcrumb>
      </div>
      <div class="equipment">
        <div class="fl equipmentType">
          设备类型
          <span style="color:red;">*</span>
        </div>
        <div class="fl equipmentInput">
          <el-select
            v-model="optionsValue"
            @change="select(optionsValue)"
            placeholder="请选择"
            style="width:100%"
          >
            <el-option
              v-for="item in options"
              :key="item.deviceType"
              :label="item.typeName"
              :value="item.deviceType"
            ></el-option>
          </el-select>
        </div>
      </div>
      <div class="equipment">
        <div class="fl equipmentType">
          探测器类型
          <span style="color:red;">*</span>
        </div>
        <div class="fl equipmentInput">
          <el-select
            v-model="detectorValue"
            @change="selDetector(detectorValue)"
            placeholder="请选择"
            style="width:100%"
          >
            <el-option
              v-for="item in detectors"
              :key="item.detectorType"
              :label="item.typeName"
              :value="item.detectorType"
            ></el-option>
          </el-select>
        </div>
      </div>
      <div class="equipment">
        <div class="fl equipmentType">
          设备信息
          <span style="color:red;">*</span>
        </div>
        <div class="fl lh showEquipment">{{selectTypeName}}</div>
        <div v-if="routeParam.type" class="fl lh">等{{defaultChecked.length}}个设备</div>
        <div v-else class="fl lh">等{{selectTypeList.length}}个设备</div>
        <div class="fr">
          <el-button @click.native.prevent="selectEquipment()" type="text" size="medium">选择设备</el-button>
        </div>
      </div>
      <div class="equipment">
        <div class="fl equipmentType">
          选择文件
          <span style="color:red;">*</span>
        </div>
        <div class="fl lh oldFile" v-show="routeParam.type">
          {{routeParam.upgradeFile}}
        </div>
        <div class="fl lh">
          <input type="file" name="file" @change="fileUpload" id="createFile" style="width:200px">
          <!-- multiple -->
        </div>
      </div>
      <div class="equipment">
        <div class="fl">备注</div>
        <div class="fr">
          <textarea name id cols="40" rows="6" v-model="desc"></textarea>
        </div>
      </div>
      <div class="equipment">
        <div class="fl" style="margin-top:30px;margin-left:120px;">
          <el-button type="primary" @click="cancel()">取消</el-button>
          <el-button type="primary" @click="save()">保存</el-button>
        </div>
      </div>
      <el-dialog title="选择设备" :visible.sync="dialogVisible" width="30%" @open="callback">
        <el-tree
          v-if="routeParam.type"
          :props="defaultProps"
          :load="loadTree"
          :highlight-current="defaultProps.highlight"
          :default-expanded-keys="defaultExpanded"
          :default-checked-keys="defaultChecked"
          node-key="eqId"
          ref="tree"
          accordion
          lazy
          show-checkbox
        ></el-tree>
        <el-tree
          v-else
          :data="dataList"
          :props="defaultProps"
          :load="loadTree"
          :highlight-current="defaultProps.highlight"
          ref="tree"
          accordion
          lazy
          show-checkbox
        ></el-tree>
        <!-- :default-checked-keys="['0E2B0003']" node-key="siteCode" -->
        <!-- @node-click="projectNodeClick" -->
        <div  slot="footer" class="dialogBtn">
          <el-button type="primary" @click="dialogVisible = false">取消</el-button>
          <el-button type="primary" @click="getCheckedKeys()">保存</el-button>
        </div>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import http from "../../../common/http.js";
import bus from "../../../common/bus";
import axios from "axios";
export default {
  name: "newAdd",
  data() {
    return {
      siteList: [],
      routeParam: {},
      upload_url: "aaa",
      dialogVisible: false,
      dataList: [],
      headers: {
        Authorization: "Bearer " + sessionStorage.getItem("token")
      },
      defaultProps: {
        // children: "equipmentList",
        label: "treeLabel",
        isLeaf: "leaf",
        highlight: true,
        node: {},
        resolve: {},
        isFirst: true
      },
      optionsValue: null,
      options: [],
      optionType: "",
      detectorValue: null,
      detectors: [],
      detectorType: "",

      isAuto: false,
      selectTypeList: [],
      desc: "",
      selectTypeName: "",
      uppgradeFiles: "",
      defaultExpanded: [],
      defaultChecked: [],
      currentNode: {},
      currentResolve: '',
    };
  },
  created: function() {
    this.routeParam = this.$route.params;
    console.log(this.$route.params, "shiwo");
    this.getDetector()
    if(this.routeParam.type){
      this.getEquipType(this.routeParam.deviceType);
      this.desc = this.routeParam.memo;
      this.getAlreadyData()
    }else{
      this.getEquipType();
    }
    bus.$on("initTree", () => {
      let url = this.querySiteListUrl;
      let data = {};
      this.$axios.post(url, data).then(
        res => {
          this.siteList = res.data.getsiteList;
          console.log(this.siteList);
          for (let i = 0; i < this.siteList.length; i++) {
            if (this.siteList[i].siteName) {
              this.siteList[i].treeLabel = this.siteList[i].siteName;
            } else {
              this.siteList[i].treeLabel = this.siteList[i].siteCode;
            }
            console.log(this.siteList[i].siteName.length);
            this.siteList[i].isDevice = false;
          }
          this.defaultProps.node.childNodes = [];
          this.defaultProps.resolve(this.siteList);
        },
        err => {}
      );
    });
  },
  beforeDestroy() {
    bus.$off("initTree");
  },
  /*方法体*/
  methods: {
    selectEquipment() {
      this.dialogVisible = true;
    },
    callback() {
      console.log('我被打开了')
      this.loadTree(this.currentNode, this.currentResolve)
    },
    
    getCheckedKeys() {
      let that = this;
      let idAry = [];
      console.log(this.$refs.tree.getCheckedNodes());
      let selectData = this.$refs.tree.getCheckedNodes();
      selectData.map(function(value, index) {
        if (value.id == undefined) {
          console.log("表头");
        } else {
          idAry.push({ id: value.id });
          that.selectTypeName = value.treeLabel;
        }
      });
      this.selectTypeList = idAry;
      this.defaultChecked = idAry;
      console.log(this.selectTypeList)
      console.log(idAry)
      this.dialogVisible = false;
    },
    cancel() {
      this.$router.go(-1);
    },
    loadTree(node, resolve) {
      console.log(this.optionType,this.detectorType,11);
      //判断初次生成保存参数
      if (this.defaultProps.isFirst) {
        this.defaultProps.node = node;
        this.defaultProps.resolve = resolve;
        this.defaultProps.isFirst = false;
      }
      this.defaultProps.node = node;
      this.defaultProps.resolve = resolve
      this.currentNode = node,
      this.currentResolve = resolve
      let treeNode = node;
      if (treeNode.level === 0) {
        //this.siteList = this.getProjectList();
        let url = this.querySiteListUrl;
        let data = {
          detectorType: this.detectorType,
          deviceType: this.optionType
        };
        this.$axios.post(url, data).then(
          res => {
            if (res.data) {
              // this.siteList = [] //每次都先置空
              this.siteList = res.data.data.siteList;
              for (let i = 0; i < this.siteList.length; i++) {
                //若存在项目备注则显示，不存在则显示项目编号
                // this.$set(this.siteList[i], "edit", true);
                if (this.siteList[i].siteName) {
                  this.siteList[i].treeLabel = this.siteList[i].siteName;
                } else {
                  this.siteList[i].treeLabel = this.siteList[i].siteCode;
                }
                this.siteList[i].eqId = this.siteList[i].siteCode
                this.siteList[i].isDevice = false;
              }
            } else {
              console.log(this.siteList);
              this.siteList = [];
            }
            console.log(this.siteList);
            
            // if(type > 1){
            //   this.defaultProps.node.childNodes = [];
            //   this.defaultProps.resolve(this.siteList);
            // }else{
            //   return resolve(this.siteList);
            // }
            this.defaultProps.node.childNodes = [];
            this.defaultProps.resolve(this.siteList);
          },
          err => {}
        );
      } else if (treeNode.level === 1) {
        // this.getTreeNode(treeNode.data.siteCode);
        let url = this.queryEquipInfoUrl;
        let data = {
          type: this.detectorType,
          deviceType: this.optionType,
          siteCode: treeNode.data.siteCode
        };
        this.$axios.post(url, data).then(
          res => {
            console.log(res.data.data);
            let len;
            if (res.data.data.equipList.length == 0) {
              len = [];
            } else {
              len = res.data.data.equipList[0].equipmentList;
              for (let i = 0; i < len.length; i++) {
                len[i].treeLabel = parseInt(len[i].deviceCode, 16) + '-' + (parseInt(len[i].lineCode) + 1) + '-' + len[i].addr;
                len[i].eqId = len[i].id
                len[i].leaf = true;
                len[i].isDevice = true;
              }
            }

            return resolve(len);
          },
          err => {}
        );
      }
    },
    //选择设备类型
    select(type) {
      this.optionType = type;
      console.log(this.optionType);
      // this.getEquipList(this.optionType);
      // this.loadTree(this.currentNode, this.currentResolve,'two')
    },
    //选择探测器
    selDetector(type) {
      this.detectorType = type
    },
    save() {
      console.log(this.filesUrl);
      let url;
      let data = {
        deviceType: this.optionType,
        uppgradeFile: this.uppgradeFiles,
        memo: this.desc,
        hixentArcUpgradeDeviceVoList: this.selectTypeList
      };
      if (this.routeParam.type == 1) {
        url = this.updataUpgradeUrl;
        data.id = this.routeParam.id;
      } else {
        url = this.addUpgradeUrl;
      }
      // let dataset = JSON.stringify(data)
      console.log(data);
      if (this.selectTypeList.length == 0) {
        this.$commonFn.showTip("请选择设备信息", 3);
      } else {
        this.$axios.post(url, data).then(res => {
          console.log(res.data);
          this.upgradeList = res.data;
          console.log(res.data.message);
          let currType;
          if (res.data.status == 0) {
            currType = 3;
          } else {
            currType = 1;
          }
          this.$commonFn.showTip(res.data.message, currType);
          this.$router.go(-1);
        });
      }
    },
    //获取设备类型数据
    getEquipType(type) {
      let url = this.queryEquipUrl;
      this.apiPost(url).then(res => {
        this.options = res.data.typeList;
        console.log(res.data.typeList)
        if(type == 1){
          this.optionType = type;
          this.optionsValue = type.toString();
        }else{
          this.optionType = res.data.typeList[0].deviceType;
          this.optionsValue = res.data.typeList[0].deviceType;
        }
        console.log(this.options)
      });
    },
    //获取探测器数据
    getDetector() {
      let url = this.detectorUrl;
      let data = {};
      this.apiPost(url).then(res => {
        this.detectors = res.data.detectorList;
        this.detectorType = res.data.detectorList[0].detectorType
        this.detectorValue = res.data.detectorList[0].detectorType;
        console.log(this.detectors)
        console.log(res.data)
      })
    },
    //获取已勾选数据
    getAlreadyData() {
      let url = this.getAlreadyUrl;
      let data = {
        id: this.routeParam.id
      }
      this.$axios.post(url,data).then(res => {
        let data = res.data.data.siteWithDevice;
        let selData = [];
        let selAry = Object.values(data);
        // selAry[0].map(function(value, index){
        //   selData.push(value.eqId)
        // })
        for(let i = 0; i < selAry.length; i++){
          selAry[i].map(function(value, index){
            selData.push(value.eqId)
          })
        }
        this.defaultExpanded = Object.keys(data)
        this.defaultChecked = selData;
        this.selectTypeName = parseInt(selAry[0][0].deviceCode, 16)
        console.log(selAry,selData)
        console.log(res.data,'666')
      })
    },
    //获取设备信息数据
    getEquipList(type) {
      let url = this.queryEquipInfoUrl;
      let data = {
        deviceType: type,
        detectorType: type
      };
      this.$axios.post(url, data).then(res => {
        this.dataList = res.data.data.equipList;
        console.log(this.dataList, "555");
      });
    },
    fileUpload(e) {
      let that = this;
      let url = this.uploadFileUrl;
      let dom = document.getElementById("createFile");
      let files = dom.files;
      console.log(files, this.uppgradeFiles);
      let formData = new FormData();
      for (let i = 0; i < files.length; i++) {
        formData.append("file", files[i]);
      }
      axios({
        method: "post",
        url: url,
        data: formData,
        headers: {
          "Content-Type": "multipart/form-data"
        }
      }).then(function(res) {
        that.uppgradeFiles = res.data.data;
      });
    },
    removeImg(file) {
      console.log(file.url);
    },
    beforeUpload(file) {
      this.uploadForm.append("imgFile", file);
      return false;
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
.oldFile{
  width: 64px;
  margin-right: 10px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>


