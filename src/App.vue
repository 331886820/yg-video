	<template>
		<div>
			<hkVideo :props-data="hkList.camera01"></hkVideo>
			<hkVideo :props-data="hkList.camera02"></hkVideo>
			<!--无视窗个数限制，更多视频组件-->
			<!-- <hkVideo :props-data="hkList.camera03"></hkVideo> -->
		</div>
	</template>

	<script>
		//  https://juejin.cn/post/7369783680425852938
		import hkVideo from "./package/yg-video/hkvideo.vue";
		var szPassword = 'yao123456',szUsername = 'admin';
		export default {
			name: 'App',
			components: {
				hkVideo
			},
			data() {
				return {
					hkList: {
						camera01: {
							szIP: "192.168.1.218", //IP地址
							szPort: "80", //端口号
							szUsername: szUsername,//账号
							szPassword: szPassword,//密码
							iChannelID: 5,// 通道ID
							loadingTime: "1000",//加载多个视频建议间隔4S左右
							width: "428",//视频窗口宽度
							height: "329",//视频窗口高度
							type: "camera01",// 自定义参数
							id: "divPlugin1"//DOM结构树--多个ID相同会重叠视窗
						},
						camera02: {
							szIP: "192.168.1.218", //IP地址
							szPort: "80", //端口号
							szUsername: szUsername,
							szPassword: szPassword,
							iChannelID: 1,
							loadingTime: "5000",
							width: "428",
							height: "330",
							type: "camera02",
							id: "divPlugin2"
						},
					}
				}

			}
		}
	</script>

	<style>
		#app {
			font-family: Avenir, Helvetica, Arial, sans-serif;
			-webkit-font-smoothing: antialiased;
			-moz-osx-font-smoothing: grayscale;
			text-align: center;
			color: #2c3e50;
			margin-top: 60px;
		}
	</style>