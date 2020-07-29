<template>
	<scroll-view @scrolltolower="handleToLower" scroll-y class="category_view" v-if="category.length > 0">
		<view class="home_category">
			<navigator class="cate_item" v-for="item in category" :key="item.id" :url="`/pages/imgCategory/index?id=` + item.id">
				<image :src="item.cover" mode="aspectFill"></image>
				<view class="cate_name">{{ item.name }}</view>
			</navigator>
		</view>
	</scroll-view>
</template>

<script>
import { uniSegmentedControl } from '@dcloudio/uni-ui';

export default {
	data() {
		return {
			category: []
		};
	},

	mounted() {
		uni.setNavigationBarTitle({
			title: '分类'
		});
		this.getList();
	},
	methods: {
		getList() {
			this.request({
				url: 'http://157.122.54.189:9088/image/v1/vertical/category'
			}).then(result => {
				this.category = result.data.res.category;
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.category_view {
	height: calc(100vh - 36px);
}

.home_category {
	padding-top: 20rpx;
	display: flex;
	justify-content: center;
	flex-wrap: wrap;
	.cate_item {
		position: relative;
		width: 33.3333%;
		display: block;
		image {
			width: 100%;
			height: 280rpx;
			display: block;
		}

		.cate_name {
			position: absolute;
			top: 90%;
			left: 50%;
			transform: translate(-50%, -90%);
			font-weight: 600;
			font-size: 40rpx;
			color: #ffffff;
			background-image: linear-gradient(to right top, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0));
		}
	}
}
</style>
