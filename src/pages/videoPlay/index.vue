<template>
	<view class="video_play">
		<image :src="videoObj.img"></image>
		<!-- 工具栏开始 -->
		<view class="video_tool">
			<view :class="['iconfont', muted ? 'icon-shengyinjingyin' : 'icon-shengyinkai']" @click="handleShengyin"></view>
			<view class="iconfont icon-zhuanfa"><button open-type="share"></button></view>
		</view>
		<!-- 工具栏结束 -->

		<!-- 视频开始 -->
		<view class="video_wrap"><video :src="videoObj.video" objectFit="fill" :muted="muted"></video></view>
		<!-- 视频结束 -->

		<!-- 下载开始 -->
		<view class="download"><view class="download_btn" @click="handleDownload">下载高清</view></view>
		<!-- 下载结束 -->
	</view>
</template>

<script>
export default {
	data() {
		return {
			videoObj: {},
			muted: false
		};
	},
	onLoad() {
		this.videoObj = getApp().globalData.video;
	},
	methods: {
		// 控制视频声音
		handleShengyin() {
			this.muted = !this.muted;
		},
		
		// 下载视频
		async handleDownload(){
			await uni.showLoading({
				title:"正在下载..."
			});
			const {tempFilePath} = (await uni.downloadFile({url:this.videoObj.video}))[1];
			uni.saveVideoToPhotosAlbum({
				filePath:tempFilePath
			});
			uni.hideLoading();
			await uni.showToast({
				title:"下载成功"
			})
		}
	}
};
</script>

<style lang="scss" scoped>
.video_play {
	position: relative;
	image {
		position: absolute;
		width: 100vw;
		height: 100vh;
		z-index: -1;
		filter: blur(20px);
	}
	.video_tool {
		position: relative;
		display: flex;
		justify-content: flex-end;
		align-items: center;
		height: 80rpx;
		padding-top: 20rpx;
		.iconfont {
			width: 60rpx;
			height: 60rpx;
			font-size: 50rpx;
			color: #ffffff;
			background-color: rgba($color: #000000, $alpha: 0.2);
			border: 2rpx solid #ffffff;
			border-radius: 30rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			margin-right: 20rpx;
		}
		.icon-zhuanfa{
			position: relative;
			button{
				position:absolute;
				width: 100%;
				height: 100%;
				opacity: 0;
			}
		}
	}

	.video_wrap {
		display: flex;
		justify-content: center;
		align-items: center;
		padding-top: 30rpx;
		video {
			border: 2rpx solid #ffffff;
			width: 90vw;
			height: 80vh;
		}
	}

	.download {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 80rpx;
		padding-top: 30rpx;
		.download_btn {
			display: flex;
			justify-content: center;
			align-items: center;
			background-color: rgba($color: #000000, $alpha: 0.2);
			color: #ffffff;
			font-size: 32rpx;
			font-weight: 500;
			width: 360rpx;
			height: 80%;
			border: 2rpx solid #ffffff;
			border-radius: 32rpx;
		}
	}
}
</style>
