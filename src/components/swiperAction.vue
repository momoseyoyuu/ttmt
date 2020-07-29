<template>
	<view @touchstart="handleTouchstart" @touchend="handleTouchend"><slot></slot></view>
</template>

<script>
export default {
	data() {
		return {
			// 按下的时间和坐标
			startTime: 0,
			startX: 0,
			startY: 0
		};
	},

	methods: {
		handleTouchstart(event) {
			// 获取手指按下屏幕时的时间和坐标
			this.startTime = Date.now();
			this.startX = event.changedTouches[0].clientX;
			this.startY = event.changedTouches[0].clientY;
		},
		handleTouchend(event) {
			// 获取手指离开屏幕时的时间和坐标
			const endTime = Date.now();
			const endX = event.changedTouches[0].clientX;
			const endY = event.changedTouches[0].clientY;

			// 判断按下的时长是否合法
			if (endTime - this.startTime > 2000) {
				return;
			}
			// 滑动的方向
			let direction = '';
			// 判断用户滑动的距离是否合法
			if (Math.abs(endX - this.startX) > 10 && Math.abs(endY - this.startY) < 10) {
				direction = endX - this.startX > 0 ? 'right' : 'left';
			} else {
				return;
			}
			this.$emit('swiperAction', { direction });
		}
	}
};
</script>

<style lang="scss" scoped></style>
