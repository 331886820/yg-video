	<template>
		<div>
			<hkVideo :props-data="hkList.camera01"></hkVideo>
			<hkVideo :props-data="hkList.camera02"></hkVideo>
		</div>
	</template>

	<script>
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
							szUsername: szUsername,
							szPassword: szPassword,
							iChannelID: 5,
							loadingTime: "1000",
							width: "428",
							height: "329",
							type: "camera01",
							id: "divPlugin1"
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