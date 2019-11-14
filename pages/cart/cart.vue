<template>
	<view>
		<cu-custom bgColor="bg-gradual-blue" :isBack="false">
		    <block slot="backText">返回</block>
		    <block slot="content">地图</block>
		</cu-custom>
		<!-- #ifdef MP-WEIXIN -->
			<button class="" @tap="wxLogin">微信登录</button>
			<button class="" @tap="checkLogin">微信登录检测</button>
			<button type="primary" open-type="getUserInfo" @tap="shouquan">获取用户信息</button>
		<!-- #endif -->
		<button type="primary" @tap="userLogin">微信登录1</button>
		<view class="page-body">
			<view class="page-section page-section-gap">
				<map v-bind:style="{width: '100%',height: mapHeight}" @tap="mapTabClick" :latitude="latitude" :longitude="longitude" :markers="covers"></map>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				mapWidth: '100%',
				mapHeight: '',
				title: 'map',
				latitude:39.909,
				longitude:116.39742,
				covers: [{
					latitude: 39.909,
					longitude: 116.39742,
					iconPath: '../../static/temp/c1.png',
					width: 20,
					height: 20,
					callout:{
						content: '测试点位',
						color: '#1cc09f',
						fontSize: 14,
						borderRadius: 5,
						bgColor: '#dcdcdc',
						padding: 5,
						display: 'BYCLICK',
						textAlign: 'center'
					},
					
				}, {
					latitude: 39.90,
					longitude: 116.39,
					iconPath: '../../static/temp/c2.png',
					width: 20,
					height: 20,
				}]
			}
		},
		methods:{
			userLogin(){
				uni.login({
					provider: 'weixin',
					success(loginRes) {
						// 获取用户信息
						uni.getUserInfo({
						  provider: 'weixin',
						  lang:"zh_CN",
						  success: function (infoRes) {
							console.log('用户昵称为：' + infoRes.userInfo.nickName);
						  },
						  fail(err) {
						  	console.log(err);
						  }
						});
					}
				})
			},
			wxLogin:function(){
				uni.login({
				  provider: 'weixin',
				  success: function (loginRes) {
				    console.log(loginRes.code);
				  }
				});
			},
			checkLogin(){
				let _this = this;
				uni.checkSession({
					success() {
						console.log("ok");
						// session_key 未过期，并且在本生命周期一直有效
					},
					fail() {
						// session_key 已经失效，需要重新执行登录流程
						_this.wxLogin() // 重新登录
						// console.log('expire');
					}
				})
			},
			shouquan(){
				let _this = this;
				uni.getSetting({
					success(res) {
						if (!res.authSetting["scope.userInfo"]) {  //3.1 每次进入程序判断当前是否获得授权，如果没有就去获得授权，如果获得授权，就直接获取当前地理位置
							uni.authorize({
								scope:"scope.userInfo",
								success() {
									_this.getUserInfo()
								},
								fail(err) {
									console.log(err)
								}
							})
						}else{
							console.log(2)
							_this.getUserInfo()
						}
					},
					fail(err) {
						console.log(err);
					}
				})
				
			},
			getUserInfo(){
				uni.getUserInfo({
					provider: 'weixin',
					lang:"zh_CN",
					success(res) {
						console.log(res)
					},
					fail(Error) {
						console.log(Error);
					}
				})
			},
			// 地图点击事件
			mapTabClick(event){
				// console.log(event);
			}
		},
		onReady() {
			let _this = this;
			uni.getSystemInfo({
			    success: function (res) {
					_this.mapHeight = res.windowHeight + 'px';
			        console.log(res.model);
			        console.log(res.pixelRatio);
					// console.log(res.screenWidth);
					// console.log(res.screenHeight);
			        console.log(res.windowWidth);
			        console.log(res.windowHeight);
					console.log(res.windowTop);
					console.log(res.windowBottom);
					console.log(res.statusBarHeight);
			        console.log(res.language);
			        console.log(res.version);
					console.log(res.system);
			        console.log(res.platform);
					console.log(res.fontSizeSetting);
					console.log(res.SDKVersion);
			    }
			});
		},
		onLoad() {
			
		},
		onShow() {
			// uni.showLoading({
			// 	mask:true,
			// 	title:'加载中'
			// })
			// setTimeout(function() {
			// 	uni.hideLoading();
			// }, 1000);
		}
	}
</script>

<style scoped>

</style>