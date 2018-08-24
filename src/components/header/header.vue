<template>
	<div class="header">
		<div class="content-wrapper">
			<!--头像-->
			<div class="avatar">
				<img width="64" height="64" :src="seller.avatar" />
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div class="support" v-if="seller.supports">
					<span :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
			<div class="support-count" v-if="seller.supports" @click="showDetail">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<!--公告-->
		<div class="bulletin-wrapper" @click="showDetail">
			<!--这两个span必须紧挨着，不然会产生span之间的默认间隙-->
			<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<!--背景-->
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%" />
		</div>
		<!--弹出层，包含两个大的div-->
		<transition name='fade'>
			<div class="detail" v-show="detailShow">
				<div class="detail-wrapper clearfix">
					<!--弹出层实体内容-->
					<div class="detail-main">
						<h1 class="name">{{seller.name}}</h1>
						<!--评分星数-->
						<div class="star-wrapper">
							<star :size="48" :score="seller.score"></star>
						</div>
						<!--标题-->
						<div class="title">
							<div class="line"></div>
							<div class="text">优惠信息</div>
							<div class="line"></div>
						</div>
						<!--标签-->
						<ul v-if="seller.supports" class="supports">
							<li class="support-item" v-for="(item,index) in seller.supports">
								<span class="icon" :class="classMap[seller.supports[index].type]"></span>
								<span class="text">{{seller.supports[index].description}}</span>
							</li>
						</ul>
						<div class="title">
							<div class="line"></div>
							<div class="text">商家公告</div>
							<div class="line"></div>
						</div>
						<div class="bulletin">
							<p class="content">{{seller.bulletin}}</p>
						</div>
					</div>
				</div>
				<div class="detail-close" @click="hide">
					<i class="icon-close"></i>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	import star from '../star/star';

	export default {
		props: {
			seller: {}
		},
		components: {
			star
		},
		data() {
			return {
				detailShow: false
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
		},
		methods: {
			showDetail() {
				this.detailShow = true
			},
			hide() {
				console.log(1)
				this.detailShow = false
			}
		}
	}
</script>

<style scoped>
	@import url("../../../static/css/common.css");
	.header {
		color: white;
		position: relative;
		background: rgba(7, 17, 27, 0.5);
		overflow: hidden;
	}
	
	.content-wrapper {
		padding: 24px 12px 18px 24px;
		font-size: 0px;
		position: relative;
	}
	
	.avatar {
		display: inline-block;
		font-size: 14px;
		vertical-align: top
	}
	
	.avatar>img {
		border-radius: 2px;
	}
	
	.content {
		margin-left: 16px;
		display: inline-block;
		font-size: 14px;
	}
	
	.title {
		margin: 2px 0 8px 0;
	}
	
	.brand {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('brand@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.name {
		margin-left: 16px;
		font-size: 16px;
		line-height: 18px;
		font-weight: bold;
	}
	
	.description {
		margin-bottom: 10px;
		line-height: 12px;
		font-size: 12px;
	}
	
	.support .icon {
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 4px;
		background-size: 12px;
		background-repeat: no-repeat;
	}
	
	.decrease {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('decrease_1@2x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.discount {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('discount_1@2x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.guarantee {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('guarantee_1@2x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.invoice {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('invoice_1@2x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.special {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('special_1@2x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.support span:first-of-type {
		display: inline-block;
		vertical-align: top;
		background-size: 10px 10px;
		width: 10px;
		height: 10px;
	}
	
	.text {
		font-size: 10px;
		display: inline-block;
		height: 10px;
		line-height: 10px;
		vertical-align: top;
	}
	
	.support-count {
		width: 20px;
		position: absolute;
		right: 12px;
		bottom: 18px;
		padding: 0 12px 0px 8px;
		height: 24px;
		line-height: 24px;
		border-radius: 14px;
		background: rgba(0, 0, 0, 0.2);
		text-align: center;
	}
	
	.support-count>.count {
		font-size: 10px;
		vertical-align: top;
	}
	
	.icon-keyboard_arrow_right {
		font-size: 10px;
		position: relative;
	}
	
	.icon-keyboard_arrow_right:before {
		content: ">";
		position: absolute;
		font-style: normal;
		margin-left: 2px;
	}
	
	.bulletin-wrapper {
		height: 28px;
		line-height: 28px;
		padding: 0 22px 0 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		position: relative;
		background: rgba(7, 17, 27, 0.2);
		padding-bottom: 2px
	}
	
	.bulletin-title {
		display: inline-block;
		width: 22px;
		height: 12px;
		background-image: url('bulletin@2x.png');
		background-size: 22px 12px;
		background-repeat: no-repeat;
		vertical-align: middle
	}
	
	.bulletin-text {
		display: inline-block;
		font-size: 10px;
		margin: 0px 4px;
	}
	
	.background {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		filter: blur(10px)
	}
	
	.detail {
		position: fixed;
		z-index: 10;
		width: 100%;
		height: 100%;
		overflow: auto;
		filter: blur(0px);
		background: rgba(7, 17, 27, .8);
		top: 0;
		left: 0;
	}
	
	.fade-enter-active,
	.fade-leave-active {
		transition: all 0.5s
	}
	
	.fade-enter,
	.fade-leave-to {
		opacity: 0;
		background: rgba(7, 17, 27, 0);
	}

	
	.detail-wrapper {
		min-height: 100%;
		width: 100%;
	}
	
	.detail-main {
		margin-top: 64px;
		/*padding-bottom是必须的,这个就是关闭标志到底部之间的距离*/
		padding-bottom: 64px;
	}
	
	.detail-close {
		position: relative;
		width: 32px;
		height: 32px;
		margin: -64px auto 0 auto;
		clear: both;
		font-size: 32px;
		text-align: center;
	}
	
	.icon-close:before {
		content: "x";
		font-style: normal;
	}
	
	.name {
		line-height: 16px;
		font-weight: 700;
		text-align: center;
	}
	
	.star-wrapper {
		margin-top: 18px;
		padding: 2px 0;
		text-align: center;
	}
	
	.detail-main .title {
		display: flex;
		width: 80%;
		margin: 30px auto 24px auto;
	}
	
	.line {
		flex: 1;
		position: relative;
		top: -6px;
		border-bottom: 1px solid rgba(255, 255, 255, 0.2);
	}
	
	.detail-main .text {
		/*padding: 0 12px;*/
		font-size: 14px;
		font-weight: 700;
	}
	
	.supports {
		width: 80%;
		margin: 0 auto;
	}
	
	.support-item {
		padding: 0 12px;
		margin-bottom: 12px;
		font-size: 0;
	}
	
	.support-item:last-of-type {
		margin-bottom: 0;
	}
	
	.support-item>icon {
		display: inline-block;
		width: 16px;
		height: 16px;
		vertical-align: top;
		margin-right: 6px;
		background-size: 16px 16px;
		background-repeat: no-repeat;
	}
	
	.support-item>.text {
		display: inline-block;
		line-height: 16px;
		font-size: 12px;
		margin-left: 4px;
	}
	
	.bulletin {
		width: 80%;
		margin: 0 12px;
		line-height: 24px;
		font-size: 12px;
	}
</style>