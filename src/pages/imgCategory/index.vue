<template>
	<view class="category_tab">
		<view class="category_tab_title">
			<view class="title_inner">
				<uni-segmented-control
					:current="current"
					:values="items.map(v => v.title)"
					@clickItem="onClickItem"
					style-type="text"
					active-color="#d4237a"
				></uni-segmented-control>
			</view>
			<view class="iconfont icon-sousuo"></view>
		</view>
		<scroll-view @scrolltolower="handleToLower" scroll-y class="category_view" v-if="vertical.length > 0">
			<view class="category_tab_content">
				<view class="cate_item" v-for="item in vertical" :key="item.id"><image :src="item.thumb" mode="widthFix"></image></view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
import { uniSegmentedControl } from '@dcloudio/uni-ui';
export default {
	components: {
		uniSegmentedControl
	},
	data() {
		return {
			items: [{ title: '最新', order: 'new' }, { title: '热门', order: 'hot' }],
			current: 0,
			params: {
				limit: 30,
				skip: 0,
				order: 'new'
			},
			id: 0,
			vertical: [],
			hasMore: true
		};
	},
	onLoad(options) {
		this.id = options.id;
		this.getList();
	},
	methods: {
		onClickItem(e) {
			if (this.current !== e.currentIndex) {
				this.current = e.currentIndex;
				this.params.order = this.items[this.current].order;
				this.params.skip=0;
				this.vertical=[];
				this.hasMore=true;
				this.getList();
			}
		},
		getList() {
			this.request({
				url: `http://157.122.54.189:9088/image/v1/vertical/category/` + this.id + `/vertical`,
				data: this.params
			}).then(result => {
				if(result.data.res.vertical.length===0){
					this.hasMore=false;
					uni.showToast({
						title:"没有更多数据了",
						icon:"none"
					})
					return;
				}
				this.vertical = this.vertical.concat(result.data.res.vertical);
			});
		},
		// 加载下一页数据
		handleToLower() {
			if (this.hasMore) {
				this.params.skip += this.params.limit;
				this.getList();
			}else{
				uni.showToast({
					title:"没有更多数据了",
					icon:"none",
				})
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.category_view {
	height: calc(100vh - 36px);
}

.category_tab {
	.category_tab_title {
		position: relative;
		.title_inner {
			width: 65%;
			margin: 0 auto;
		}
	}
	.icon-sousuo {
		position: absolute;
		top: 50%;
		transform: translate(-50%, -50%);
		right: 5%;
	}
	.category_tab_content {
		display: flex;
		flex-wrap: wrap;
		.cate_item {
			width: 33.3333%;
			display: block;
			image {
				display: block;
				width: 100%;
				height: 100%;
			}
		}
	}
}
</style>
