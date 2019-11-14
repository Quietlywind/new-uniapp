<template>
	<view class="container">
		<!-- <image class="logo" src="/static/logo.png" @tap="ceshi()"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view class="title">{{nowDate}}</view> -->
		<!-- 头部轮播 -->
		<view class="carousel-section">
			<view class="titleNview-placing"></view>
			<view class="titleNview-background" :style="{backgroundColor:titleNViewBackground}"></view>
			<swiper class="carousel"  :interval="5000" :duration="800" autoplay circular indicator-dots indicator-active-color="#1cc09f" @change="swiperChange">
				<swiper-item v-for="(item, index) in carouselList" :key="index" class="carousel-item" @tap="navToDetailPage({title: '轮播测试'})">
					<image :src="item.src" />				
				</swiper-item>
			</swiper>
			<!-- 自定义swiper指示器 -->
			<view class="swiper-dots">	
				<text class="num">{{swiperCurrent+1}}</text>
				<text class="sign">/</text>
				<text class="num">{{swiperLength}}</text>
			</view>
		</view>
		<!-- 分类 -->
		<view class="cate-section">
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c3.png"></image>
				<text>环球美食</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c5.png"></image>
				<text>个护美妆</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c6.png"></image>
				<text>营养保健</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c7.png"></image>
				<text>家居厨卫</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c2.png"></image>
				<text>婴幼儿品</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c4.png"></image>
				<text>厨卫茶具</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c1.png"></image>
				<text>健康卫生</text>
			</view>
			<view class="cate-item" @tap="cateClick">
				<image src="/static/temp/c8.png"></image>
				<text>冲调饮品</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				titleNViewBackground: '',
				title: 'Hello World',
				swiperCurrent: 0,
				swiperLength: 0,
				carouselList: [
					{
						src: "/static/temp/banner1.jpg",
						background: "rgb(239, 100, 94, .8)",
					},
					{
						src: "/static/temp/banner3.jpg",
						background: "rgb(203, 87, 60)",
					},
					{
						src: "/static/temp/banner2.jpg",
						background: "rgb(205, 215, 218)",
					},
					{
						src: "/static/temp/banner4.jpg",
						background: "rgb(183, 73, 69)",
					}
				],
				src: '',
			}
		},
		onLoad() {
			this.loadData()
			// console.log(new Date().getMonth() + 1)
		},
		methods: {
			loadData() {
				let ceshi = 0;
				this.titleNViewBackground = this.carouselList[0].background;
				this.swiperLength = this.carouselList.length;
			},
			//轮播图切换修改背景色
			swiperChange(e) {
				const index = e.detail.current;
				this.swiperCurrent = index;
				this.titleNViewBackground = this.carouselList[index].background;
			},
			navToDetailPage(val) {
				uni.showModal({
					content: val.title,
					showCancel: false,
					confirmColor: '#1cc09f',
				});
				// uni.navigateTo({
				//     url: '/pages/details/details',
				// });	
			},
			takePhoto() {
				// #ifdef APP-PLUS || H5
				uni.chooseImage({
				    count: 6, //默认9
				    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				    sourceType: ['album'], //从相册选择
				    success: function (res) {
						uni.showModal({
							content: '1121',
							showCancel: false,
							confirmColor: '#1cc09f',
						})
				        this.src = JSON.stringify(res.tempFilePaths);
				    }
				});
				// #endif
				// #ifdef MP-WEIXIN
					const ctx = uni.createCameraContext();
					console.log(ctx)
					ctx.takePhoto({
						quality:'high',
						success: (res) => {
							console.log(res)
							this.src = res.tempImagePath
						}
					})
				// #endif
				
			},
			cateClick() {
				uni.navigateTo({
				    url: '/pages/details/details',
				});
			}
		},
		onReady() {
			// console.log(new Date().getHours())
		},
		computed:{
			nowDate () {
				return new Date().getHours()
			}
		}
	}
</script>

<style lang="scss">
 	// .content {
 	// 	display: flex;
 	// 	flex-direction: column;
 	// 	align-items: center;
 	// 	justify-content: center;
 	// 	background-color: #DCDCDC;
 	// }
 
 // 	.logo {
 // 		height: 200upx;
 // 		width: 200upx;
 // 		margin-top: 200upx;
 // 		margin-left: auto;
	// 	margin-right: auto;
 // 		margin-bottom: 50upx;
 // 	}
 // 
 // 	.text-area {
 // 		display: flex;
 // 		justify-content: center;
	// }
 // 
 // 	.title {
	// 	font-size: 36upx;
	// 	color: #8f8f94;
	// }
	/* #ifdef MP */
	page {
		.cate-section{
			position:relative;
			z-index:5;
			border-radius:16upx 16upx 0 0;
			margin-top:-20upx;
		}
		.carousel-section{
			padding: 0;
			.titleNview-placing {
				padding-top: 0;
				height: 0;
			}
			.carousel{
				.carousel-item{
					padding: 0;
				}
			}
			.swiper-dots{
				left:45upx;
				bottom:40upx;
			}
		}
	}
	/* #endif */
	page {
		background: #f5f5f5;
	}
	.carousel-section{
		position: relative;
		padding-top: 10px;
		.titleNview-placing{
			height: var(--status-bar-height);
			// padding-top: 44px;
			padding-top: 14px;
			box-sizing: content-box;
		}
		.titleNview-background{
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			// height: 426upx;
			height: 360upx;
			transition: .4s;
		}
	}
	.carousel{
		width: 100%;
		height: 350upx;
		.carousel-item{
			width: 100%;
			height: 100%;
			padding: 0 28upx;
			overflow: hidden;
		}
		image{
			width: 100%;
			height: 100%;
			border-radius: 19upx;
		}
	}
	.swiper-dots {
		display: flex;
		position: absolute;
		left: 60upx;
		bottom: 15upx;
		width: 72upx;
		height: 36upx;
		background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMgAAABkCAYAAADDhn8LAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTMyIDc5LjE1OTI4NCwgMjAxNi8wNC8xOS0xMzoxMzo0MCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6OTk4MzlBNjE0NjU1MTFFOUExNjRFQ0I3RTQ0NEExQjMiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6OTk4MzlBNjA0NjU1MTFFOUExNjRFQ0I3RTQ0NEExQjMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTcgKFdpbmRvd3MpIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0E3RUNERkE0NjExMTFFOTg5NzI4MTM2Rjg0OUQwOEUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0E3RUNERkI0NjExMTFFOTg5NzI4MTM2Rjg0OUQwOEUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4Gh5BPAAACTUlEQVR42uzcQW7jQAwFUdN306l1uWwNww5kqdsmm6/2MwtVCp8CosQtP9vg/2+/gY+DRAMBgqnjIp2PaCxCLLldpPARRIiFj1yBbMV+cHZh9PURRLQNhY8kgWyL/WDtwujjI8hoE8rKLqb5CDJaRMJHokC6yKgSCR9JAukmokIknCQJpLOIrJFwMsBJELFcKHwM9BFkLBMKFxNcBCHlQ+FhoocgpVwwnv0Xn30QBJGMC0QcaBVJiAMiec/dcwKuL4j1QMsVCXFAJE4s4NQA3K/8Y6DzO4g40P7UcmIBJxbEesCKWBDg8wWxHrAiFgT4fEGsB/CwIhYE+AeBAAdPLOcV8HRmWRDAiQVcO7GcV8CLM8uCAE4sQCDAlHcQ7x+ABQEEAggEEAggEEAggEAAgQACASAQQCCAQACBAAIBBAIIBBAIIBBAIABe4e9iAe/xd7EAJxYgEGDeO4j3EODp/cOCAE4sYMyJ5cwCHs4rCwI4sYBxJ5YzC84rCwKcXxArAuthQYDzC2JF0H49LAhwYUGsCFqvx5EF2T07dMaJBetx4cRyaqFtHJ8EIhK0i8OJBQxcECuCVutxJhCRoE0cZwMRyRcFefa/ffZBVPogePihhyCnbBhcfMFFEFM+DD4m+ghSlgmDkwlOgpAl4+BkkJMgZdk4+EgaSCcpVX7bmY9kgXQQU+1TgE0c+QJZUUz1b2T4SBbIKmJW+3iMj2SBVBWz+leVfCQLpIqYbp8b85EskIxyfIOfK5Sf+wiCRJEsllQ+oqEkQfBxmD8BBgA5hVjXyrBNUQAAAABJRU5ErkJggg==);
		background-size: 100% 100%;
	
		.num {
			width: 36upx;
			height: 36upx;
			border-radius: 50px;
			font-size: 24upx;
			color: #fff;
			text-align: center;
			line-height: 36upx;
		}
	
		.sign {
			position: absolute;
			top: 0;
			left: 50%;
			line-height: 36upx;
			font-size: 12upx;
			color: #fff;
			transform: translateX(-50%);
		}
	}
	.cate-section {
		display: flex;
		justify-content: space-around;
		align-items: center;
		flex-wrap:wrap;
		padding: 20upx 20upx; 
		background: #fff;
		.cate-item {
			display: flex;
			flex-direction: column;
			align-items: center;
			font-size: $font-sm + 2upx;
			color: $font-color-dark;
			min-width: 25%;
			padding: 10upx;
		}
		/* 原图标颜色太深,不想改图了,所以加了透明度 */
		image {
			width: 88upx;
			height: 88upx;
			margin-bottom: 14upx;
			border-radius: 50%;
			opacity: .7;
			box-shadow: 4upx 4upx 20upx rgba(250, 67, 106, 0.3);
		}
	}
</style>
