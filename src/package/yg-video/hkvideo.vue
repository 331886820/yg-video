<template>
    <div style="position: relative;height: 100%;text-align: center;">
      <div :id="newId" class="plugin"></div>
    </div>
</template>

<script>
export default {
  name:'hkVideo',
  inheritAttrs: false,
  props:{
    propsData:{
      type: Object,
	  default:{},
      require:true
    }
  },
  watch:{
    propsData: {
    deep: true,
    immediate: true,
    handler(newVal, oldVal) {
      if(newVal){
        this.ygCamera = newVal
        this.newId = newVal.id;
        let t = newVal.loadingTime || 1000
        setTimeout(() => {
           this.initS();
        },t)
      }else{
        this.ygCamera = {
            szIP: "192.168.1.218", //IP地址
            szPort: "80", //端口号
            szUsername: "admin", //用户名
            szPassword: "123456", //管理员密码
            iChannelID: 5,
            loadingTime: "3000",
            width: "800",
            height: "360",
            type: "camera01",
            id:"divPlugin1"
        }
        this.initS();
      }
    }
   }
  },
  data() {
    return {
      ygCamera:{},
      newId:null
    };
  },
  mounted() {
    console.log('插件下载地址：/webSDKPlugin/HCWebSDKPlugin.exe');
    // this.initS();
    window.addEventListener("resize", this.viewReload)
  },
  methods: {
	handleXXX(){
		var szDeviceIdentify = this.ygCamera.szIP+'_'+this.ygCamera.szPort;
		this.$emit('desPlug',szDeviceIdentify);
	},
   async initS() {
      let that = this;
      // 初始化
     await WebVideoCtrl.I_InitPlugin({
        bWndFull: true, //是否支持单窗口双击全屏，默认支持 true:支持 false:不支持
        iWndowType: 1,// 画面分割数 1 就是1*1   2就是2*2
        cbDoubleClickWnd: function(iWndIndex, bFullScreen) {
						var szInfo = "当前放大的窗口编号：" + iWndIndex;
						if (!bFullScreen) {
							szInfo = "当前还原的窗口编号：" + iWndIndex;
						}
						console.log(szInfo);
					},
        cbInitPluginComplete: function () {
          WebVideoCtrl.I_InsertOBJECTPlugin(that.newId).then(
            () => {
              // 检查插件是否最新
              WebVideoCtrl.I_CheckPluginVersion().then((bFlag) => {
                if (bFlag) {
                  let str = "检测到新的插件版本，双击开发包目录里的HCWebSDKPlugin.exe升级！";
                  that.$message.error(str);
                  console.log(str);
                }
              });
            },
            () => {
              let str1 = "插件初始化失败，请确认是否已安装插件；如果未安装，请双击开发包目录里的HCWebSDKPlugin.exe安装！";
                that.$message.error(str1);
                console.log(str1);
            }
          );
        },
      });
      setTimeout(() => {
        let cw = Math.round(document.body.clientWidth/1920);
        let ch = Math.round(document.body.clientHeight/1080);
        let width = parseInt((this.ygCamera.width*cw),10);
        let height = parseInt((this.ygCamera.height*ch),10);
        if(height <= 200){ height = 200; }
        if(width <= 200){ width = 200; }
        // 对窗口大小重新规划
        WebVideoCtrl.I_Resize(
          width,
          height
        );
        this.setVideoWindowResize(that.newId,width,height);
        this.clickLogin();
      }, 2000);
    },
    // 根据设备宽高和windowchange设置窗口大小
    setVideoWindowResize(pid,width,height){
       document.getElementById(pid).style.width = width + 'px';
       document.getElementById(pid).style.height = height + 'px';
    },
    // 登录
    clickLogin() {
      let that = this;
      var szIP = this.ygCamera.szIP,
        szPort = this.ygCamera.szPort,
        szUsername = this.ygCamera.szUsername,
        szPassword = this.ygCamera.szPassword;
        const iRet =  WebVideoCtrl.I_Login(szIP, 1, szPort, szUsername, szPassword, {
          timeout: 3000,
          success: function (xmlDoc) {
            setTimeout(function () {
                setTimeout(function() {
                  that.getChannelInfo();
                }, 1000);
                that.getDevicePort();
            }, 10);
          },
          error: function (err) {
            console.log("登录-err===>",err);
          },
        });

        if (iRet === -1) {
            console.log(this.szDeviceIdentify + " 已登录过！");
            // 登录过直接进行预览
            this.clickStartRealPlay();
        }
    },

    // 获取端口
    getDevicePort() {
      var szDeviceIdentify = this.ygCamera.szIP;
      if (null == szDeviceIdentify) {
        return;
      }
      WebVideoCtrl.I_GetDevicePort(szDeviceIdentify).then((oPort) => {
        console.log("152获取端口",oPort);
      });
    },
    // 获取通道
    async getChannelInfo() {
      let that = this;
      var szDeviceIdentify = this.ygCamera.szIP+'_'+this.ygCamera.szPort;

      if (null == szDeviceIdentify) {
        return;
      }
      console.log('szDeviceIdentify==============>',szDeviceIdentify);
      // 模拟通道
	  
	 // try{
	 // 	WebVideoCtrl.I_GetAnalogChannelInfo(szDeviceIdentify, {
	 // 	  success: function (xmlDoc) {
	 // 	    that.clickStartRealPlay();
	 // 	  },
	 // 	  error: function (oError) {
	 // 	    console.log(szDeviceIdentify + "模拟通道", oError.errorCode, oError.errorMsg);
	 // 	  }
	 // 	});
	 // }catch(e){
	 // 	console.log('模拟通道报错就注释这里，因为没有模拟通道');
	 // } 
      

      // 数字通道
    WebVideoCtrl.I_GetDigitalChannelInfo(szDeviceIdentify, {
        success: function (xmlDoc) {
          that.clickStartRealPlay(); 
        },
        error: function (oError) {
          console.log(szDeviceIdentify + " 数字通道", oError.errorCode, oError.errorMsg);
        }
    });
    },
    // 开始预览
    clickStartRealPlay(iStreamType) {
      let that = this;
      var g_iWndIndex = 0;
      var oWndInfo = WebVideoCtrl.I_GetWindowStatus(g_iWndIndex) || null;
      var szDeviceIdentify = this.ygCamera.szIP+'_'+this.ygCamera.szPort,
        iChannelID = this.ygCamera.iChannelID,// 5=>4楼测试电子    2=>4楼前台    1=>4楼后门
        bZeroChannel = false;
      if ("undefined" === typeof iStreamType) {
        iStreamType = 1;
      }
      if (null == szDeviceIdentify) {
        return;
    }
      var startRealPlay = function () {
        WebVideoCtrl.I_StartRealPlay(szDeviceIdentify, {
          iStreamType: iStreamType,
          iChannelID: iChannelID,
          bZeroChannel: bZeroChannel,
          success: function () {
            WebVideoCtrl.I_Logout(szDeviceIdentify)
            console.log("开始预览成功！");
          },
          error: function (oError) {
            // that.$message.error(szDeviceIdentify+"开始预览失败！");
            console.log(szDeviceIdentify+"开始预览失败！");
          },
        });
      };

      if (oWndInfo != null) {// 已经在播放了，先停止
        WebVideoCtrl.I_Stop({
            success: function () {
                startRealPlay();
            }
        });
      } else {
          startRealPlay();
      }
    },
    // 停止预览
    clickStopRealPlay() {
      WebVideoCtrl.I_Stop({
        success: function () {
          console.log("停止预览成功！");
        },
        error: function (oError) {
          console.log(" 停止预览失败！");
        },
      });
    },
    loginOut(){
      WebVideoCtrl.I_Logout(this.szDeviceIdentify);
    },
    stopAllPlay(){
      WebVideoCtrl.I_StopAllPlay();
    },
    breakDom(){
      WebVideoCtrl.I_DestroyPlugin(this.szDeviceIdentify)
    },
    viewReload(){
      window.location.reload()	
    },
    /** * 播放器大小 */
    setStyle(){
      let elem = document.getElementsByClassName('bot-one');
      let w = elem.offsetWidth,h = elem.offsetHeight;
      document.getElementById(this.ygCamera.id).style.width = width+'px'
      document.getElementById(this.ygCamera.id).style.height = height+'px'
    }
  },
  beforeDestroy(){
    this.loginOut();
    this.stopAllPlay();
    window.removeEventListener(this.viewReload);
  },
  destroyed() {
     setTimeout(() => {
        this.breakDom();
     },100)
  },
};
</script>

<style scoped lang="less">
.plugin {
  height: 319px;
  width: 428px;
  margin-top: 10px;
}
#divPlugin2{
   margin-top:-10px;
}
#video-img2{
  margin-top: 20px;
}
</style>
