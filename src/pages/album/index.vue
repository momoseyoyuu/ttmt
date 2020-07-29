<template>
	<view>
		<!-- 专辑背景开始 -->
		<view class="album_bg">
			<image :src="album.cover" mode="widthFix"></image>
			<view class="album_info">
				<!-- <view class="album_name">{{ album.name }}</view> -->
				<view class="album_name">测试{{ album.name }}</view>
				<view class="album_btn">关注专辑</view>
			</view>
		</view>
		<!-- 专辑背景结束 -->
		<!-- 专辑作者开始 -->
		<view class="album_author">
			<view class="album_author_info">
				<image :src="album.user.avator" mode="widthFix"></image>
				<view class="album_author_name">测试{{ album.user.name }}</view>
			</view>
			<view class="album_author_desc">
				<text>
					本课程将会借助uni-app框架的能力带你使用现在前端最火的vue语法来实现一个精美图片微信小程序应用，课程中包含大量目前前端必备的技能，如vue，微信小程序，组件封装，移动端手势封装，数据分页，axios，monent，flex布局，sass，视频播放，视频下载等等功能。{{
						album.dec
					}}
				</text>
			</view>
		</view>
		<!-- 专辑作者结束 -->
		<view class="album_list">
			<view class="album_item" v-for="(item, index) in wallpaper" :key="item.id">
				<get-detail :list="wallpaper" :index="index"><image :src="item.thumb + item.rule.replace('$<Height>', 360)" mode="aspectFill"></image></get-detail>
			</view>
		</view>
	</view>
</template>

<script>
import getDetail from '@/components/getDetail';
export default {
	components: { getDetail },
	data() {
		return {
			params: {
				limit: 30,
				oreder: 'new',
				skip: 0,
				first: 1
			},
			id: -1,
			album: {},
			wallpaper: [
				{ id: 1, thumb: '1', rule: '1' },
				{ id: 2, thumb: '1', rule: '1' },
				{ id: 3, thumb: '1', rule: '1' },
				{ id: 4, thumb: '1', rule: '1' },
				{ id: 5, thumb: '1', rule: '1' },
				{ id: 6, thumb: '1', rule: '1' },
				{ id: 7, thumb: '1', rule: '1' },
				{ id: 8, thumb: '1', rule: '1' },
				{ id: 9, thumb: '1', rule: '1' },
				{ id: 10, thumb: '1', rule: '1' }
			],
			hasMore: true
		};
	},

	computed: {
		// 当最后一行的图片不满三张时，自动添加白色图片
		addPicture() {
			let picture = { id: 9999, thumb: '1', rule: '1' };
			if (this.wallpaper.length % 3 != 0) {
				let j = this.wallpaper.length % 3;
				for (let i = 0; i < 3 - j; i++) {
					this.wallpaper.push(picture);
				}
			}
		}
	},

	// 加载页面数据
	onLoad(options) {
		this.id = options.id;
		this.getList();
	},

	// 滚动到底部时加载更多图片
	onReachBottom() {
		if (this.hasMore) {
			this.params.first = 0;
			this.params.skip += this.params.limit;
			this.getList();
		} else {
			uni.showToast({
				title: '没有更多数据啦~',
				icon: 'none'
			});
		}
	},

	methods: {
		getList() {
			this.request({
				url: 'http://157.122.54.189:9088/image/v1/wallpaper/album/' + this.id + '/wallpaper',
				data: this.params
			}).then(result => {
				console.log(result);
				if (Object.keys(this.album).length === 0) {
					this.album = result.data.res.album;
				}
				if (result.data.res.wallpaper.length === 0) {
					this.hasMore = false;
					uni.showToast({
						title: '已经没有更多数据啦~',
						icon: 'none'
					});
					return;
				}
				this.wallpaper = this.wallpaper.concat(result.data.res.wallpaper);
			});
		}
	}
};
</script>

<style lang="scss" scoped>
// 专辑背景
.album_bg {
	position: relative;
	width: 100%;
	background-color: #000000;
	image {
		width: 100%;
	}

	.album_info {
		position: absolute;
		width: 100%;
		left: 0;
		bottom: 0;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 80rpx;
		color: #ffffff;
		.album_name {
			margin-left: 15rpx;
			font-size: 40rpx;
		}
		.album_btn {
			background-color: $color;
			width: 152rpx;
			height: 56rpx;
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 10rpx;
			margin-right: 15rpx;
		}
	}
}

// 专辑作者
.album_author {
	padding: 10rpx;
	.album_author_info {
		padding: 10rpx 0;
		display: flex;
		image {
			width: 50rpx;
			height: 50rpx;
			border-radius: 50%;
			background-color: #000000;
		}
		.album_author_name {
			color: #000000;
			margin-left: 15rpx;
		}
	}

	.album_author_desc {
		padding: 10rpx;
	}
}

.album_list {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	// align-content: space-between;
	
	margin-bottom: 0rpx;
	.album_item:nth-child(odd) {
		width: 33.3333%;
		// border: 2rpx solid #ffffff;
		display: block;
		image {
			display: block;
			background-color: #876543;
			width: 100%;
			height: 160rpx;
		}
	}
	.album_item:nth-child(even) {
		width: 33.3333%;
		// border: 2rpx solid #ffffff;
		display: block;
		image {
			display: block;
			background-color: #000000;
			width: 100%;
			height: 160rpx;
		}
	}
}
</style>
