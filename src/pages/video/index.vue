<template>
	<view>
		<view class="video_tab">
			<view class="video_tab_title">
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
			<view class="video_tab_content">
				<view v-if="current < 4"><videoMain  :urlobj="{url:items[current].url,params:items[current].params}"></videoMain></view>
				<view v-if="current === 4"><videoCategory></videoCategory></view>
			</view>
		</view>
	</view>
</template> 

<script>
import { uniSegmentedControl } from '@dcloudio/uni-ui';
import { videoMain } from './video-main/index.vue';
import { videoCategory } from './video-category/index.vue';
export default {
	components: {
		uniSegmentedControl,
		videoMain,
		videoCategory
	},
	data() {
		return {
			items: [
				{
					title: '推荐',
					url:"http://157.122.54.189:9088/videoimg/v1/videowp/featured",
					params:{
						limit:30,
						skip:0,
						order:"hot"
					}
				},
				{
					title: '娱乐',
					url:"http://157.122.54.189:9088/videoimg/v1/videowp/category/59b25abbe7bce76bc834198a",
					params:{
						limit:30,
						skip:0,
						order:"new"
					}
				},
				{
					title: '最新',
					url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
					params:{
						limit:30,
						skip:0,
						order:"new"
					}
				},
				{
					title: '热门',
					url:"http://157.122.54.189:9088/videoimg/v1/videowp/videowp",
					params:{
						limit:30,
						skip:0,
						order:"hot"
					}
				},
				{
					title: '分类',
					url:"http://157.122.54.189:9088/videoimg/v1/videowp/category",
					params:{}
				}
			],
			current: 0
		};
	},
	onLoad() {
		uni.setNavigationBarTitle({
			title: this.items[this.current].title
		});
	},
	methods: {
		onClickItem(e) {
			if (this.current !== e.currentIndex) {
				this.current = e.currentIndex;
				uni.setNavigationBarTitle({
					title: this.items[this.current].title
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.video_tab {
	.video_tab_title {
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
}

.video_tab_content {
	padding-top: 20rpx;
}
</style>
