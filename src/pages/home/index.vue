<template>
	<view>
		<view class="home_tab">
			<view class="home_tab_title">
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
			<view class="home_tab_content">
				<view v-if="current === 0"><homeRecommend></homeRecommend></view>
				<view v-if="current === 1"><homeCategory></homeCategory></view>
				<view v-if="current === 2"><homeNew></homeNew></view>
				<view v-if="current === 3"><homeAlbum></homeAlbum></view>
			</view>
		</view>
	</view>
</template>

<script>
import { uniSegmentedControl } from '@dcloudio/uni-ui';
import { homeAlbum } from './home-album';
import { homeCategory } from './home-category';
import { homeNew } from './home-new';
import { homeRecommend } from './home-recommend';
export default {
	components: {
		homeAlbum,
		homeCategory,
		homeNew,
		homeRecommend,
		uniSegmentedControl
	},
	data() {
		return {
			items: [
				{
					title: '推荐'
				},
				{
					title: '分类'
				},
				{
					title: '最新'
				},
				{
					title: '专辑'
				}
			],
			current: 0
		};
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
	},
	onLoad() {
		uni.setNavigationBarTitle({
			title: this.items[this.current].title
		});
	}
};
</script>

<style lang="scss">
.home_tab {
	.home_tab_title {
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
</style>
