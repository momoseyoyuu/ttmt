<template>
	<scroll-view scroll-y class="album_view">
		<!-- 轮播图开始 -->
		<view class="album_swiper">
			<swiper autoplay indicator-dots circular>
				<swiper-item v-for="item in banner" :key="item.id"><image :src="item.thumb"></image></swiper-item>
			</swiper>
		</view>
		<!-- 轮播图结束 -->
		<view class="album_list">
			<navigator class="album_item" v-for="item in album" :key="item.id" :url="'/pages/album/index?id=' + item.id">
				<view class="album_img"><image :src="item.cover" mode="aspectFill"></image></view>
				<view class="album_info">
					<view class="album_nd">
						<view class="album_name">{{ item.name }}</view>
						<view class="album_desc">{{ item.desc }}</view>
					</view>
					<view class="album_btn"><view class="album_attention">+ 关注</view></view>
				</view>
			</navigator>
		</view>
	</scroll-view>
</template>

<script>
export default {
	data() {
		return {
			params: {
				limit: 30,
				order: 'new',
				skip: 0
			},
			banner: [],
			album: [],
			hasMore: true
		};
	},
	mounted() {
		this.getList();
	},
	methods: {
		getList() {
			this.request({
				url: 'http://157.122.54.189:9088/image/v1/wallpaper/album',
				data: this.params
			}).then(result => {
				if (this.banner.length === 0) {
					this.banner = result.data.res.banner;
				}
				if (result.data.res.album.length === 0) {
					this.hasMore = false;
					return;
				}
				this.album = this.album.concat(result.data.res.album);
			});
		},
		// 滚动条触底事件
		handleToLower() {
			if (this.hasMore) {
				this.params.skip += this.params.limit;
				this.getList();
			}else{
				uni.showToast({
					title:"没有更多数据啦~",
					icon:"none"
				})
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.album_view {
	// 注意calc()括号里面的空格
	height: calc(100vh - 36px);
}
.album_swiper {
	swiper {
		height: calc(750rpx / 2.3);
		image {
			width: 100%;
			height: 100%;
		}
	}
}

.album_list {
	padding: 10rpx;
	.album_item {
		padding: 10rpx 0;
		display: flex;
		border-bottom: 1rpx solid rgba($color: #cccccc, $alpha: 0.5);
		.album_img {
			flex: 1;
			padding: 10rpx;
			image {
				width: 240rpx;
				height: 240rpx;
			}
		}

		.album_info {
			flex: 2;
			padding: 0 10rpx;
			overflow: hidden;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
			.album_nd {
				.album_name {
					font-size: 30rpx;
					color: #000000;
					padding: 10rpx 0;
				}

				.album_desc {
					padding: 10rpx 0;
					font-size: 24rpx;

					text-overflow: ellipsis;
					overflow: hidden;
					white-space: nowrap;
				}
			}
			.album_btn {
				padding: 10rpx;
				display: flex;
				justify-content: flex-end;
				.album_attention {
					color: $color;
					border: 1rpx solid $color;
					padding: 10rpx;
				}
			}
		}
	}
}
</style>
