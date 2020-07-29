<template>
	<scroll-view @scrolltolower="handleToLower" scroll-y class="recommend_view" v-if="recommends.length > 0">
		<!-- 推荐开始 -->
		<view class="recommend_wrap">
			<navigator class="recommend_item" v-for="item in recommends" :key="item.id" :url="`/pages/album/index?id=` + item.target">
				<image mode="widthFix" :src="item.thumb"></image>
			</navigator>
		</view>
		<!-- 推荐结束 -->

		<!-- 月份开始 -->
		<view class="months_wrap">
			<view class="months_titile">
				<view class="months_titile_info">
					<view class="months_info">
						<text>{{ months.DD }}/</text>
						{{ months.MM }} 月
					</view>
					<view class="months_text">{{ months.title }}</view>
				</view>
				<view class="months_titile_more">更多 ></view>
			</view>
			<view class="months_content">
				<view class="months_item" v-for="(item, index) in months.items" :key="item.id">
					<get-detail :list="months.items" :index="index"><image :src="item.thumb + item.rule.replace('$<Height>', 360)" mode="aspectFill"></image></get-detail>
				</view>
			</view>
		</view>
		<!-- 月份结束 -->
		<!-- 热门开始 -->
		<view class="hots_wrap">
			<view class="hots_title"><text>热门</text></view>
			<view class="hots_content">
				<view class="hots_item" v-for="item in hots" :key="item.id"><image :src="item.thumb" mode="aspectFill"></image></view>
			</view>
		</view>
		<!-- 热门结束 -->
	</scroll-view>
</template>

<script>
import moment from 'moment';
import getDetail from '@/components/getDetail';
export default {
	components: {
		getDetail
	},
	data() {
		return {
			// 推荐列表
			recommends: [],
			// 月份列表
			months: [],
			// 热门列表
			hots: [],
			params: {
				limit: 30,
				order: 'hot',
				skip: 0
			},
			hasMore: true
		};
	},
	mounted() {
		this.getList();
	},
	methods: {
		// 获取接口的数据
		getList() {
			this.request({
				url: 'http://157.122.54.189:9088/image/v3/homepage/vertical',
				data: this.params
			}).then(result => {
				// 判断还有没有下一页数据
				if (result.data.res.vertical.length === 0) {
					this.hasMore = false;
					return;
				}
				if (this.recommends.length === 0) {
					// 获取推荐列表
					this.recommends = result.data.res.homepage[1].items;
					// 获取月份列表
					this.months = result.data.res.homepage[2];
					// 使用moment.js将月份时间戳改成22号/07月格式
					this.months.MM = moment(this.months.stime).format('MM');
					this.months.DD = moment(this.months.stime).format('DD');
				}
				// 拼接热门列表
				this.hots = this.hots.concat(result.data.res.vertical);
				console.log(this.recommends);
			});
		},

		// 滚动条触底事件
		handleToLower() {
			if (this.hasMore) {
				this.params.skip += this.params.limit;
				this.getList();
			} else {
				uni.showToast({
					title: '没有更多数据啦~',
					icon: 'none'
				});
			}
		}
	}
};
</script>

<style lang="scss">
.recommend_view {
	// 注意calc()括号里面的空格
	height: calc(100vh - 36px);
}
.recommend_wrap {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-evenly;
	padding-top: 20rpx;
	.recommend_item {
		display: block;
		width: 50%;
		image {
			width: 100%;
			display: block;
		}
	}
}

.months_wrap {
	.months_titile {
		display: flex;
		justify-content: space-between;
		padding: 20rpx;
		.months_titile_info {
			color: $color;
			font-size: 30rpx;
			font-weight: 600;
			display: flex;
			.months_info {
				font-size: 24rpx;
				text {
					font-size: 30rpx;
				}
			}
			.months_text {
				font-size: 32rpx;
				color: #666666;
				margin-left: 30rpx;
			}
		}
		.months_titile_more {
			font-size: 24rpx;
			color: $color;
			margin: auto 0;
		}
	}
	.months_content {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		.months_item {
			display: block;
			width: 33.3333%;
			image {
				display: block;
				width: 100%;
			}
		}
	}
}

.hots_wrap {
	.hots_title {
		padding: 20rpx;
		text {
			border-left: 20rpx solid $color;
			padding-left: 20rpx;
			font-size: 34rpx;
			font-weight: 600;
		}
	}
	.hots_content {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		.hots_item {
			width: 33.3333%;
			display: block;
			image {
				display: block;
				width: 100%;
			}
		}
	}
}
</style>
