<template>
	<view>
		<cu-custom bgColor="bg-gradual-green" :isBack="false">
		    <block slot="backText">返回</block>
		    <block slot="content">分类</block>
		</cu-custom>
		<view class="content">
			<view>
				<text>{{longitude}}</text>
			</view>
			<view>
				<text>{{latitude}}</text>
			</view>
		</view>
		<!-- #ifdef MP-WEIXIN -->
			<view>
				<!-- <camera device-position="back" flash="off" @error="error" style="width: 100%; height: 300px;"></camera> -->
				<button class="bottom" type="primary" @click="takePhoto">拍照</button>
				<view>预览</view>
				<image mode="widthFix" :src="src"></image>
			</view>
			<view class="cu-bar bg-white margin-top">
				<view class="action">
					图片上传
				</view>
				<view class="action">
					{{imgList.length}}/4
				</view>
			</view>
			<view class="cu-form-group">
				<view class="grid col-4 grid-square flex-sub">
					<view class="bg-img" v-for="(item,index) in imgList" :key="index" @tap="ViewImage" :data-url="imgList[index]">
					 <image :src="imgList[index]" mode="aspectFill"></image>
						<view class="cu-tag bg-red" @tap.stop="DelImg" :data-index="index">
							<text class='cuIcon-close'></text>
						</view>
					</view>
					<view class="solids" @tap="ChooseImage" v-if="imgList.length<4">
						<text class='cuIcon-cameraadd'></text>
					</view>
				</view>
			</view>
		<!-- #endif -->
		<!-- <button type="primary" @tap="shouquan">获取用户信息</button> -->
	</view>
</template>

<script>
	export default {
		data () {
			return {
				ceshi: '测试列表',
				longitude: '',
				latitude: '',
				imgList: [],
				src: '',
			}
		},
		onLoad() {
		},
		methods: {
			getAuthorizeInfo (a="scope.userLocation") {
				var _this=this;
				uni.authorize({
					scope: a,
					success() { //1.1 允许授权
						_this.getLocationInfo();
					},
					fail(){    //1.2 拒绝授权
						uni.showModal({
							content:'你拒绝了授权，无法获得周边信息',
							confirmColor: '#1cc09f',
							showCancel: false,
						})
						// console.log("你拒绝了授权，无法获得周边信息")
					}
				})
			},
			getLocationInfo(){
				let that = this;
				uni.getLocation({
				    type: 'wgs84',
				    success: function (res) {
						that.longitude = res.longitude;
						that.latitude = res.latitude;
						// #ifdef MP-WEIXIN
							uni.request({
								header:{
									"Content-Type": "application/text"
								},
								url:'http://apis.map.qq.com/ws/geocoder/v1/?location='+res.longitude.toString()+','+res.latitude.toString()+'&key=MVGBZ-R2U3U-W5CVY-2PQID-AT4VZ-PDF35',
								success(res) {
									console.log(res.data);
									if(res.statusCode===200){
										console.log("获取中文街道地理位置成功")
									}else{
										console.log("获取信息失败，请重试！")
									}
								}
							})
						// #endif
				    }
				});
			},
			isGetLocation(a="scope.userLocation"){
				let _this = this;
				uni.getSetting({
					fail(err) {
						console.log(err);
					},
					success(res) {
						if (!res.authSetting[a]) {  //3.1 每次进入程序判断当前是否获得授权，如果没有就去获得授权，如果获得授权，就直接获取当前地理位置
							_this.getAuthorizeInfo()
						}else{
							_this.getLocationInfo()
						}
					}
				})
			},
			takePhoto() {
				// const ctx = uni.createCameraContext();
				// ctx.takePhoto({
				// 	quality: 'high',
				// 	success: (res) => {
				// 		console.log(res)
				// 		this.src = res.tempImagePath
				// 	}
				// });
				let _this = this;
				uni.chooseImage({
					count: 4,
					sizeType:["original","compressed"],
					sourceType: ['album'],
					success: (res) => {
						// console.log(JSON.stringify(res.tempFilePaths));
						console.log(res.tempFilePaths[0]);
						this.src = res.tempFilePaths[0];
						// var tempFilePaths = res.tempFilePaths;
						// uni.saveFile({
						// 	tempFilePath: tempFilePaths[0],
						// 	success: function (res) {
						// 		var savedFilePath = res.savedFilePath;
						// 		_this.src = savedFilePath;
						// 	},
						// 	fail(error) {
						// 		console.log(error);
						// 	}
						// })
					}
				})
			},
			error(e) {
				console.log(e.detail);
			},
			ChooseImage() {
				uni.chooseImage({
					count: 4, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						if (this.imgList.length != 0) {
							this.imgList = this.imgList.concat(res.tempFilePaths)
						} else {
							this.imgList = res.tempFilePaths
						}
					}
				});
			},
			ViewImage(e) {
				uni.previewImage({
					urls: this.imgList,
					current: e.currentTarget.dataset.url
				});
			},
			DelImg(e) {
				uni.showModal({
					title: '召唤师',
					content: '确定要删除这段回忆吗？',
					cancelText: '再看看',
					confirmText: '再见',
					success: res => {
						if (res.confirm) {
							this.imgList.splice(e.currentTarget.dataset.index, 1)
						}
					}
				})
			},
		},
		onReady() {
			// #ifdef MP
				this.isGetLocation()
			// #endif
			// #ifdef APP-PLUS || H5
				this.getLocationInfo()
			// #endif
		},
		computed:{
			nowDate () {
				return new Date().getHours()
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.bottom{
		border-radius: 80rpx;
		margin: 70rpx 50rpx;
		font-size: 35rpx;
	}
	image{will-change: transform}
</style>
