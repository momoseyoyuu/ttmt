<script>
// import {uniBadge} from '@dcloudio/uni-ui';
export default {
	// components: {uniBadge},
	onLaunch: function() {
		console.log('App Launch');
	},
	onShow: function() {
		console.log('App Show');
	},
	onHide: function() {
		console.log('App Hide');
	}
};
</script>

<style>
/*每个页面公共css */
@font-face {
	font-family: 'iconfont'; /* project id 1920118 */
	src: url('//at.alicdn.com/t/font_1920118_7cswsa9xd85.eot');
	src: url('//at.alicdn.com/t/font_1920118_7cswsa9xd85.eot?#iefix') format('embedded-opentype'), url('//at.alicdn.com/t/font_1920118_7cswsa9xd85.woff2') format('woff2'),
		url('//at.alicdn.com/t/font_1920118_7cswsa9xd85.woff') format('woff'), url('//at.alicdn.com/t/font_1920118_7cswsa9xd85.ttf') format('truetype'),
		url('//at.alicdn.com/t/font_1920118_7cswsa9xd85.svg#iconfont') format('svg');
}

.iconfont {
	font-family: 'iconfont' !important;
	font-size: 16px;
	font-style: normal;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

.icon-shengyinjingyin:before {
	content: '\e631';
}

.icon-shengyinkai:before {
	content: '\e630';
}

.icon-xiazai:before {
	content: '\e68b';
}

.icon-dianzan1:before {
	content: '\e61c';
}

.icon-dianzan2:before {
	content: '\e601';
}

.icon-shoucang1:before {
	content: '\e694';
}

.icon-shoucang2:before {
	content: '\e73d';
}

.icon-dianzan:before {
	content: '\e607';
}

.icon-jingyinmute33:before {
	content: '\e62f';
}

.icon-zhuanfa:before {
	content: '\e65f';
}

.icon-huo:before {
	content: '\e664';
}

.icon-fire:before {
	content: '\e78c';
}

.icon-pinglun:before {
	content: '\e648';
}

.icon-shoucang:before {
	content: '\e613';
}

.icon-jiantou:before {
	content: '\e644';
}

.icon-kouhong:before {
	content: '\e603';
}

.icon-search:before {
	content: '\e621';
}

.icon-Videorecordcamera:before {
	content: '\e9d4';
}

.icon-My:before {
	content: '\e6b3';
}

.icon-img:before {
	content: '\e61b';
}

.icon-sousuo:before {
	content: '\e618';
}
</style>
