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
		<div class="detail" v-show="detailShow"></div>
	</div>
</template>

<script>
	export default {
		props: {
			seller: {}
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
			}
		}
	}
</script>

<style>
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
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('discount_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.guarantee {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('guarantee_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.invoice {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('invoice_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.special {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('special_1@2x.png');
		background-size: 30px 18px;
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
		filter: blur(5px);
		background: rgba(7, 17, 27, .8);
		top: 0;
		left: 0;
	}
</style>