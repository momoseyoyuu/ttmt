<template>
	<view style="background-color: #e5dfea;">
		<!-- 用户系信息开始 -->
		<view class="user_info">
			<view class="user_icon"><image :src="imgDetail.user.avator" mode="widthFix"></image></view>
			<view class="user_desc">
				<view class="user_name">姓名{{ imgDetail.user.name }}</view>
				<view class="user_time">时间{{ imgDetail.user.cnTime }}</view>
			</view>
		</view>
		<!-- 用户系信息结束 -->
		<!-- 高清大图开始 -->
		<view class="high_img">
			<swiper-action @swiperAction="handleSwiperAction"><image :src="imageDetail.thumb" mode="widthFix"></image></swiper-action>
		</view>
		<!-- 高清大图结束 -->
		<!-- 点赞收藏开始 -->
		<view class="img_rank">
			<view class="rank">
				<text class="iconfont" :class="[isRankTrue ? 'icon-dianzan2' : 'icon-dianzan1']" @click="handleRank">10000{{ imgDetail.rank }}</text>
			</view>
			<view class="collect"><text class="iconfont" :class="[isCollectTrue ? 'icon-shoucang1' : 'icon-shoucang2']" @click="handleCollect">收藏</text></view>
		</view>
		<!-- 点赞收藏结束 -->
		<!-- 专辑开始 -->
		<view class="album_wrap" v-if="album.length">
			<!-- 标题 -->
			<view class="album_title">相关</view>
			<!-- 内容 -->
			<view class="album_list">
				<view class="album_item" v-for="item in album" :key="item.id">
					<view class="album_cover"><image :src="item.cover" mode="aspectFill"></image></view>
					<view class="album_info">
						<view class="album_text">专辑</view>
						<view class="album_name">测试{{ item.name }}</view>
						<text class="iconfont icon-jiantou"></text>
					</view>
				</view>
			</view>
		</view>
		<!-- 专辑结束 -->

		<!-- 最热评论开始 -->
		<view class="comment hot" v-if="hot.length">
			<view class="comment_title">
				<text class="iconfont icon-huo"></text>
				<text class="comment_text">最热评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item" v-for="item in hot" :key="item.id">
					<!-- 用户信息 -->
					<view class="comment_user">
						<!-- 用户头像 -->
						<view class="user_icon"><image :src="item.user.avatar" mode=""></image></view>
						<!-- 用户信息 -->
						<view class="user_name">
							<view class="user_nickname">姓名{{ item.user.name }}</view>
							<view class="user_time">时间{{ item.atime }}</view>
						</view>
						<view class="user_badge"><image v-for="item2 in item.user.title" :key="item2.icon" :src="item2.icon"></image></view>
					</view>
					<view class="comment_desc">
						<view class="comment_content">内容{{ item.content }}</view>
						<view class="comment_like">
							<text class="iconfont" :class="isHotRankTrue ? 'icon-dianzan2' : 'icon-dianzan1'" @click="handleHotRank">666{{ item.size }}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 最热评论结束 -->

		<!-- 最新评论开始 -->
		<view class="comment new" v-if="comment.length">
			<view class="comment_title">
				<text class="iconfont icon-pinglun"></text>
				<text class="comment_text">最新评论</text>
			</view>
			<view class="comment_list">
				<view class="comment_item" v-for="item in comment" :key="item.id">
					<!-- 用户信息 -->
					<view class="comment_user">
						<!-- 用户头像 -->
						<view class="user_icon"><image :src="item.user.avatar" mode=""></image></view>
						<!-- 用户信息 -->
						<view class="user_name">
							<view class="user_nickname">姓名{{ item.user.name }}</view>
							<view class="user_time">时间{{ item.cnTime }}</view>
						</view>
						<view class="user_badge"><image v-for="item2 in item.user.title" :key="item2.icon" :src="item2.icon"></image></view>
					</view>
					<view class="comment_desc">
						<view class="comment_content">内容{{ item.content }}</view>
						<view class="comment_like">
							<text class="iconfont" :class="isNewRankTrue ? 'icon-dianzan2' : 'icon-dianzan1'" @click="handleNewRank">666{{ item.size }}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 最新评论结束 -->

		<!-- 下载开始 -->
		<view class="download"><view class="download_btn" @click="handleDownload">下载图片</view></view>

		<!-- 下载结束 -->
	</view>
</template>

<script>
import moment from 'moment';
import swiperAction from '@/components/swiperAction';
// 设置时间的语言为中文
moment.locale('zh-cn');
export default {
	components: {
		swiperAction
	},
	data() {
		return {
			imgDetail: {},
			// 当前图片点赞
			isRankTrue: false,
			// 当前图片收藏
			isCollectTrue: false,
			// 最热评论点赞
			isHotRankTrue: false,
			// 最新评论点赞
			isNewRankTrue: false,
			// 专辑数据
			album: [{ id: 1, thumb: '1', rule: '1' }, { id: 2, thumb: '1', rule: '1' }, { id: 3, thumb: '1', rule: '1' }],
			// 最热评论
			hot: [{ id: 1, thumb: '1', rule: '1' }, { id: 2, thumb: '1', rule: '1' }, { id: 3, thumb: '1', rule: '1' }],
			// 最新评论
			comment: [{ id: 1, thumb: '1', rule: '1' }, { id: 2, thumb: '1', rule: '1' }, { id: 3, thumb: '1', rule: '1' }],
			// 图片索引号
			imgIndex: 0
		};
	},

	onLoad() {
		// console.log(getApp().globalData)
		const { imgIndex } = getApp().globalData;
		// 加载上个页面点击的图片
		this.imgIndex = imgIndex;
		this.getData();
	},
	methods: {
		// 当前图片点赞按钮
		handleRank() {
			// 切换样式
			this.isRankTrue = !this.isRankTrue;
		},

		// 当前图片收藏按钮
		handleCollect() {
			// 切换样式
			this.isCollectTrue = !this.isCollectTrue;
		},

		// 最热评论点赞按钮
		handleHotRank() {
			// 切换样式
			this.isHotRankTrue = !this.isHotRankTrue;
		},

		// 最新评论点赞按钮
		handleNewRank() {
			// 切换样式
			this.isNewRankTrue = !this.isNewRankTrue;
		},

		// 给当前页面赋值
		getData() {
			const { imgList } = getApp().globalData;
			this.imgDetail = imgList[this.imgIndex];
			// 时间格式化
			this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).format();
			// 获取图片详情id
			// 获取评论
			this.getComment(this.imgDetail.id);
		},

		// 获取评论
		getComment(id) {
			this.request({
				url: 'http://157.122.54.189:9088/image/v2/wallpaper/wallpaper/' + id + '/comment'
			}).then(result => {
				this.album = result.data.res.album;
				// 给hot数组中的对象添加一个时间属性 XXX月前
				result.res.hot.forEach(v => (v.cnTime = moment(v.atime * 1000).fromNow()));
				this.hot = result.data.res.hot;
				this.comment = result.data.res.comment;
			});
		},

		// 滑动事件
		handleSwiperAction(event) {
			// console.log(event.direction);
			// 左滑 imgIndex++
			// 右滑 imgIndex--
			const { imgList } = getApp().globalData;
			if (event.direction === 'left' && this.imgIndex < imgList.length - 1) {
				this.imgIndex++;
				this.getData();
			} else if (event.direction === 'right' && this.imgIndex > 0) {
				this.imgIndex--;
				this.getData();
			} else {
				uni.showToast({
					title: '没有数据了',
					icon: 'none'
				});
			}
		},

		// 点击下载图片
		async handleDownload() {
			// 1 将远程文件下载到小程序的内存中tempFilePath
			await uni.showLoading({
				title:"下载中"
			});
			const result1 = await uni.downloadFile({
				url: this.imgDetail.img
			});
			const { tempFilePath } = result1[1];	
			// 2 将小程序内存中的临时文件下载到本地上
			const result2 = await uni.saveImageToPhotosAlbum({
				filePath: tempFilePath
			});
			// 3 提示用户下载成功			
			uni.hideLoading();
			await uni.showToast({
				title:"下载成功",
			})
		}
	}
};
</script>

<style lang="scss" scoped>
// 用户信息
.user_info {
	display: flex;
	padding: 20rpx;
	justify-content: flex-start;
	background-color: #ffffff;
	.user_icon {
		padding: 0 20rpx 0 0;
		display: flex;
		justify-content: center;
		align-items: center;
		image {
			width: 88rpx;
			height: 88rpx;
			background-color: #000000;
			border-radius: 50%;
		}
	}
	.user_desc {
		display: flex;
		// justify-content: center;
		flex-direction: column;
		// align-items: center;
		justify-content: flex-start;
		.user_name {
			color: #000000;
			font-weight: 600;
			padding-top: 2rpx;
		}
		.user_time {
			color: #7b797c;
			font-size: 24rpx;
			padding-top: 2rpx;
		}
	}
}

// 高清大图
.high_img {
	width: 100%;
	height: 360rpx;
	// border-bottom: 6rpx solid #e5dfea;
	image {
		width: 100%;
		height: 360rpx;
		background-color: #000000;
	}
}

.img_rank {
	height: 60rpx;
	display: flex;
	justify-content: space-evenly;
	align-items: center;
	color: #666666;
	background-color: #ffffff;
	.rank {
		width: 50%;
		text-align: center;
		font-size: 24rpx;
		font-weight: 400;
	}
	.collect {
		width: 50%;
		text-align: center;
		font-size: 24rpx;
		font-weight: 400;
	}
}

.icon-dianzan2 {
	color: $color;
}

.icon-shoucang1 {
	color: $color;
}

// 专辑模块
.album_wrap {
	padding: 20rpx;
	margin: 10rpx 0;
	background-color: #ffffff;
	.album_title {
		// padding: 10rpx 0;
	}
	.album_list {
		.album_item {
			display: flex;
			padding: 10rpx 0;
			border-bottom: 3rpx solid #eeeeee;
			.album_cover {
				flex: 1;
				image {
					width: 180rpx;
					height: 180rpx;
					background-color: #000000;
				}
			}

			.album_info {
				flex: 3;
				padding-left: 20rpx;
				position: relative;
				.album_text {
					width: 100rpx;
					height: 50rpx;
					color: #ffffff;
					background-color: $color;
					display: flex;
					justify-content: center;
					align-items: center;
				}
				.album_name {
					padding: 10rpx 0;
					color: #999999;
				}
				.iconfont {
					font-size: 40rpx;
					color: #999999;
					position: absolute;
					top: 50%;
					right: 0;
					transform: translate(0, -50%);
				}
			}
		}
	}
}

// 最热评论模块
.comment {
	margin-top: 20rpx;
	background-color: #ffffff;
	.comment_title {
		padding: 15rpx;
		display: flex;
		align-items: center;
		.iconfont {
			color: #ff3344;
			font-size: 40rpx;
		}
		.comment_text {
			font-weight: 600;
			font-size: 28rpx;
			color: #666666;
			margin-left: 10rpx;
		}
	}
	.comment_list {
		.comment_item {
			border: 3rpx solid #eeeeee;
			.comment_user {
				display: flex;
				justify-content: flex-start;
				padding: 10rpx;
				.user_icon {
					width: 100rpx;
					height: 100rpx;
					border-radius: 50%;
					background-color: #000000;
					display: flex;
					justify-content: center;
					align-items: center;

					image {
						width: 90%;
						height: 90%;
					}
				}
				.user_name {
					flex: 1;
					padding-left: 10rpx;
					.user_nickname {
						color: #000000;
					}

					.user_time {
						color: #999999;
						font-size: 24rpx;
						padding: 5rpx 0;
					}
				}
				.user_badge {
					width: 40rpx;
					height: 40rpx;
					background-color: #000000;
					border-radius: 50%;
					display: inline-block;
					image {
					}
				}
			}
			.comment_desc {
				display: flex;
				padding: 10rpx;
				padding-left: 10rpx;
				.comment_content {
					flex: 1;
					padding-left: 15%;
					color: #000000;
				}
				.comment_like {
					text-align: right;
					.iconfont {
					}
				}
			}
		}
	}
}

// 最新评论模块
.new {
	.icon-pinglun {
		color: #007aff !important;
	}
}

.download {
	height: 120rpx;
	display: flex;
	align-items: center;
	justify-content: center;
	.download_btn {
		width: 75%;
		height: 65%;
		background-color: $color;
		color: #ffffff;
		border-radius: 14rpx;
		font-size: 40rpx;
		font-weight: 600;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.download_btn:active {
		background-color: #bc1f6e;
	}
}
</style>
