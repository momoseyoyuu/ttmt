<template>
	<scroll-view @scrolltolower="handleToLower" scroll-y enable-flex class="video_main">
		<view @click="handleGoVideo(item)" class="video_item" v-for="item in videowp" :key="item.id"><image :src="item.img" mode="widthFix"></image></view>
	</scroll-view>
</template>

<script>
export default {
	props: {
		urlobj: Object
	},
	data() {
		return {
			videowp: [],
			hasMore: true
		};
	},
	watch: {
		urlobj() {
			this.videowp = [];
			this.hasMore = true;
			this.getList();
		}
	},
	mounted() {
		this.getList();
	},
	methods: {
		getList() {
			this.request({
				url: this.urlobj.url,
				data: this.urlobj.params
			}).then(result => {
				if (result.data.res.videowp.length === 0) {
					this.hasMore = false;
					return;
				}
				this.videowp = this.videowp.concat(result.data.res.videowp);
			});
		},

		// 滚动条触底事件
		handleToLower() {
			if (this.hasMore) {
				this.urlobj.params.skip += this.urlobj.params.limit;
				this.getList();
			} else {
				uni.showToast({
					title: '没有更多数据啦~',
					icon: 'none'
				});
			}
		},
		
		// 图片点击事件
		handleGoVideo(item){
			getApp().globalData.video=item;
			uni.navigateTo({
				url:"/pages/videoPlay/index"
			})
		}
	}
};
</script>

<style lang="scss" scoped>
.video_main {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	height: calc(100vh - 36px);
	.video_item {
		width: 33.3333%;
		display: block;
		image {
			width: 100%;
			display: block;
		}
	}
}
</style>
